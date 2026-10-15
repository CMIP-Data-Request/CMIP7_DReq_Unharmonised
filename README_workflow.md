
# Technical Workflow for CMIP7 Unharmonised Data Request

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

MIPs are responsible for coordinating with modelling centres to carry out MIP experiments and publish the requested variables.
This repository is a technical resource to make data request information easily available to them, it does not imply coordination with modelling centres by the CMIP IPO on behalf of MIPs.

## How to create a data request

A new data request is created by submitting a pull request in this repository. The basic procedure is:

1. Clone this repo and checkout a new branch
2. Define a new DR Opportunity
3. Define any new Experiment or Variable Groups
4. Run the validation script to check technical compliance
5. Submit pull request for review

See below for explanation of each step. 


### 1. Clone the repository

```bash
git clone git@github.com:CMIP-Data-Request/CMIP7_DReq_Unharmonised.git
cd CMIP7_DReq_Unharmonised/
git checkout -b mip_request
```
replacing `mip_request` with a suitable descriptive name.


### 2. Define a new DR Opportunity

An "Opportunity" lists the variables requested from a set of experiments and explains why they're requested.
The explanation can be brief but should cover the basics and, if possible, include links to reference information (such as a MIP documentation paper).

Start by copying the Opportunity template:
```bash
cd Opportunity/
cp TEMPLATE_Opportunity.yaml mip_request.yaml
```
replacing `mip_request.yaml` with a descriptive file name. 
The Opportunity file should remain in the `Opportunity` folder.
Edit the file to describe the new Opportunity.

The Opportunity description can be brief or detailed.
For examples of Opportunities in the Harmonised DR, see `reference_Harmonised/Opportunity`.
(Some Harmonised DR Opportunities go into considerable detail but that is not mandatory for the Unharmonised DR.)
Note that in general there are several ways to view the Harmonised DR content:

- [Airtable](https://bit.ly/CMIP7-DReq-latest)
- [DR web viewer](https://cmip-data-request.github.io/cmip7-dreq-webview/latest)
- `yaml` files in `reference_Harmonised/`

The choice of viewer is solely a user preference (the content is the same).


### 3. Define any new Experiment or Variable Groups

Experiment Groups listed in the Opportunity file can be:

- from the Harmonised DR, as listed in the `reference_Harmonised/Experiment_Group` folder
- from other MIPs, as listed in the `Experiment_Group` folder
- brand new

To create a new Experiment Group, follow the example file (`Experiment_Group/example_experiment_group.yaml`) and reference the new group from the Opportunity file.
The new group's `yaml` file should remain in the `Experiment_Group` folder.

Similarly, Variable Groups listed in the Opportunity file can be existing ones (from either the Harmonised or Unharmonised DR) or new ones (following the example: `Variable_Group/example_variable_group.yaml`).
The new group's `yaml` file should remain in the `Variable_Group` folder.
Variables in a Variable Group `yaml` file should be named by their CMIP7 Compound Name (example: "atmos.uas.tavg-h10m-hxy-u.day.glb").

To help creating variable groups, [see below](#help-with-variable-groups) for some tools.


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


## Help with Variable Groups

There are several ways to disover DR variables to include in a new Variable Group.

The [DR webviewer variable search](https://cmip-data-request.github.io/cmip7-dreq-webview/variable_search.html) can be used to search for variables of interest.
They can then be listed in a Variable Group `yaml` file by their CMIP7 Compound Names (example: "atmos.uas.tavg-h10m-hxy-u.day.glb").

The Variables table in the [DR Airtable](https://bit.ly/CMIP7-DReq-latest) can be searched using Airtable's filtering/viewing options to isolate variables of interest. Variable Groups already defined in the Harmonised DR can also be viewed in the [DR Airtable](https://bit.ly/CMIP7-DReq-latest) as well as the [DR webviewer](https://cmip-data-request.github.io/cmip7-dreq-webview/latest/variable_groups.html).

The DR software has a command-line tool to produce a listing of DR variables and their metadata attributes in `json` or `csv` (spreadsheet) format:
```bash
get_variables_metadata v1.2.2.5 vars.json
```
or 
```bash
get_variables_metadata v1.2.2.5 vars.csv
```
By default this produces a list of the almost 2000 variables defined in the Harmonised DR. 
To isolate only variables of interest, the list can be filtered:
```bash
get_variables_metadata v1.2.2.5 vars.csv -f filter_variables.yaml
```
where for example the filtering `yaml` file could contain:
```yaml
frequency:
- day
- 3hr
variableRootDD:
- uas
- vas
```
to retain only `uas` and `vas` at both `day` and `3hr` frequencies.
This may be useful to produce an initial list of variables when creating a new variable group.
The list can be then be edited to choose the variables for the group.

⚠️ The inclusion of metadata in the `json` or `csv` file is for information only, to make it easier to select relevant variables. The metadata cannot be edited to change the definition of a variable (for example, its cell methods, dimensions, or CF standard name). Defining *new* variables as part of the Unharmonised DR is *in development* and not yet available.

After editing the list, the another command-line tool can be used to render it in the `yaml` format required to make a pull request in the Unharmonised repo:
```bash
make_variable_group vars.csv my_new_variable_group.yaml
```
where the output file should be some sensible descriptive name.
This produces a template `yaml` file with the names of variables listed, and also validates the input against DR reference content.
Users should fill in the rest of the Variable Group template, including specifying a priority level (High, Medium or Low) for the group.

⚠️ If a `csv` file is used, the file that is input to `make_variable_group` must have the same format as the original `csv` file: the first row gives the column headers, and subsequent rows list the variables.


## FAQ

> If this workflow doesn't work, what do I do?

Please help us improve the tools by [opening an issue](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/issues) describing the problem.

> Do I need to be a git expert to use this?

Some familiarity with git and github is assumed, but we have tried to make the tools as simple as possible while allowing for automated checks of technical validity (to ensure that requested variables and experiments confirm to CMIP7 data standards making them publishable on ESGF).
Feedback to improve the process is welcome: you can [open an issue](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/issues) or can [start a dicussion](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/discussions).

> Why is Unharmonised content stored as `yaml` files?

`yaml` files are easily readable by both humans and software.

> I don't like `yaml`, can I use spreadsheets to make a data request?

Yes, provided the information is converted into `yaml` format for the pull request. 
[See here](#help-with-variable-groups) for guidance on using the DR software to do this.

> Who reviews these pull requests?

MIP requests should be submitted with the approval of MIP leads who have registered their contact info with the CMIP IPO.
Pull requests will be reviewed for technical compliance by the WIP-Ops (WIP Operations) team, who maintain this repository.
MIP leads are responsible for the scientific validity of the request.

> Why are experiments/variables listed in groups?

This makes requests easier to read, encourages re-use of existing groups wher epossible, and helps make requests easier to understand. 
(Variable and experiment lists can get very long.)

> Can I request changes to Harmonised DR content by making a pull request in this repo?

No, the CMIP7 Harmonised DR is focused on Fast Track experiments and is finalised (subject to minor updates when errors are found). 
Find the latest version of the Harmonised DR [here](https://wcrp-cmip.org/cmip7-data-request-latest).

> How does this process differ from development of the Hamonised DR?

The Harmonised DR is targetted at the CMIP7 AFT and was developed in an extensive community consultation and review process, as described in the[Data request overview paper in CMIP7 GMD special issue](https://doi.org/10.5194/gmd-19-8053-2026) and on the [CMIP7 Data Request website](https://wcrp-cmip.org/cmip-phases/cmip7/cmip7-data-request/).
The Unharmonised DR has no such development process.
Scientific review is the responsibility of contributing MIPs, and technical review is aimed only at ensuring experiments and variables listed in a request are compliant with CMIP7 data standards so that they are publishable on ESGF.

> Is Unharmonised DR content versioned as part of the Harmonised DR versioning?

No, they are versioned separately.
This reflects their different development approaches, and also the fact that the Harmonised DR is now mostly stable pending minor technical updates only when necessary.
The Unharmonised DR, in contrast, is expected to evolve significantly, determined by MIP constributions on timelines unconstrained by the AFT.

> Does submitting a request here mean that it will become part of the CMIP8 data request?

Not necessarily, but it could improve its chances. 
While focused on CMIP7, the Unharmonised DR is also intended to smooth the path toward incorporating community MIP requests into the next iteration of the Harmonised CMIP DR.

