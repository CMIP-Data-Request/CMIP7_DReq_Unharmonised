
# Workflow for CMIP7 Unharmonised Data Request

This document explains how a MIP can create a data request for MIP-defined experiments using this git repository and the [DR Software](https://github.com/CMIP-Data-Request/CMIP7_DReq_Software).

It's assumed that the MIP has already registered with the IPO and CVs (registration links can be found [here](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Guidance_for_MIPs/)).

## What this repository does

- Gather new data requests for MIP experiments, making them easily available to modelling centres and data users
- Store data requests in a consistent format (allowing DR software tools to work with them)
- Facilitate technical checks on data request (compliance with metadata standards, etc)

## What this repository does *not* do

- Register MIPs or experiments (registration links can be found [here](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Guidance_for_MIPs/))
- Define new request variables (CMOR variables), which will be done in the **WCRP Variable Registry** currently in development
- Instigate an extensive review process as done for the community consultation that created the Harmonised DR

MIPs are responsible for coordinating with modelling centres to carry out their experiments and provide their requested variables.
This repository is simply a technical resource to make data request information easily available to them.


## How to create a data request

A new data request is created by submitting a pull request in this repository. The basic procedure is:

1. Clone this repo and checkout a new branch
2. Define a new DR Opportunity
3. Define any new Experiment or Variable Groups
4. Run the validation script to check technical compliance
5. Submit pull request for review

Further details of each step are given below.

### 1. Clone the repository

```bash
git clone git@github.com:CMIP-Data-Request/CMIP7_DReq_Unharmonised.git
cd CMIP7_DReq_Unharmonised/
git checkout -b mip_request
```
replacing `mip_request` with a suitable descriptive name.

### 2. Define a new DR Opportunity

An "Opportunity" lists the variables requested from a set of experiments, and explains why they're requested.
The explanation can be brief but should cover the basics and if possible include links to reference information (e.g., a MIP documentation paper).

Start by copying the Opportunity template:
```bash
cd Opportunity/
cp TEMPLATE_Opportunity.yaml mip_request.yaml
```
replacing `mip_request.yaml` with some brief descriptive file name. 
For examples from the Harmonised DR, see `reference_Harmonised/Opportunity`.

Edit the `yaml` file to describe the new Opportunity.
This can be brief or detailed.
Some Harmonised DR Opportunities include considerable detail, but this is not mandatory for the Unharmonised DR.

The Opportunity file should remain in the `Opportunity` folder.

### 3. Define any new Experiment or Variable Groups

Experiment Groups listed in the Opportunity file can be:
- from the Harmonised DR, as listed in the `reference_Harmonised/Experiment_Group` folder,
- from other MIPs, as listed in the `Experiment_Group` folder,
- brand new.
To create a new Experiment Group, follow the example file (`Experiment_Group/example_experiment_group.yaml`) and reference the new group from the Opportunity file.
The new group's `yaml` file should remain in the `Experiment_Group` folder.

Similarly, Variable Groups listed in the Opportunity file can be existing ones (from either the Harmonised or Unharmonised DR) or new ones (follow the example file `Variable_Group/example_variable_group.yaml`).
The new group's `yaml` file should remain in the `Variable_Group` folder.

Note there are several ways to view the Harmonised DR content:
- [Airtable](https://bit.ly/CMIP7-DReq-latest)
- [DR web viewer](https://cmip-data-request.github.io/cmip7-dreq-webview/latest)
- `yaml` files in `reference_Harmonised/`
The choice of viewer is solely a user preference.

⚠️ *In development:* tool to help create a variable group from a spreadsheet (csv file) or json listing variables metadata (which can be created by `get_variables_metadata`).

### 4. Run the validation script

This performs automated checks for techical compliance.
These include checking that:
- variables included are registered in the CVs
- experiments are registered in the CVs
- new variable or experiment groups don't duplicate existing ones

The validation script is part of the DR Software ([see here](https://github.com/CMIP-Data-Request/CMIP7_DReq_Software) for how to install it).

⚠️ *In development* For now use the `unharmonised_dev2` branch. The validation script will be updated.

To run the script:
```bash
 validate_DR_opportunity Opportunity/mip_request.yaml mip_request.json v1.2.2.5
```
If the script finishes without errors, the validation checks have passed.

### 5. Submit pull request for review

```bash
git push -u origin mip_request
```
Then navigate to the [github repo](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised) and open a pull request for this branch.
The pull request description should indicate:
- if the validation checks have passed
- MIP leads endorse the request (if they are not the submitters, their github handles should be tagged)

Once accepted and merged, the Opportunity is part of the Unharmonised DR, accesible to data producers.
Likewise any new Experiment or Variable Groups, which may potentially be re-used by other MIPs.


## FAQ

> If the workflow doesn't work, what do I do?

Please [open an issue](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/issues) describing the problem.

> Why is content stored here as `yaml` files?

`yaml` files are easily readable by both humans and software.

> I don't like `yaml`, can I use spreadsheets to make a data request?

Yes, provided the information is converted into `yaml` format for the pull request. 

⚠️ *In development: provide a tool to help with this.*

> Who reviews these pull requests?

MIP requests should be submitted with the approval of MIP leads who have registered their contact info with the CMIP IPO.
Pull requests are subject to review for technical compliance by a WIP-designated working group who maintain this repository.
MIP leads are responsible for the scientific validity of the request.

> Why are experiments/variables listed in groups?

This makes requests easier to read, encourages re-use of existing groups wher epossible, and helps make requests easier to understand. 
(Variable and experiment lists can get very long.)

> Can I request changes to Harmonised DR content by making a pull request in this repo?

No. 
The CMIP7 Harmonised DR is focused on Fast Track experiments and is finalised (subject to minor updates when errors are found). 
Find the latest version of the Harmonised DR [here](https://wcrp-cmip.org/cmip7-data-request-latest).
