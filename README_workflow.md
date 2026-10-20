
# Technical Workflow for CMIP7 Unharmonised Data Request

This document explains how a Community MIP can create a data request for its experiments or other CMIP7 experiments as part of the CMIP7 Unharmonised Data Request (DR).
The Unharmonised DR is not tied to Assessment Fast Track (AFT) timelines.

Prior to creating a data request by following the workflow described in this document, a Community MIP should already have:

- **Registered as a MIP** with the CMIP IPO
- Registered the MIP as an **activity** in the CMIP7 CVs (the `activity_id` [Global Attribute](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Global_Attributes/))
- Registered the MIP's **experiments** in the CMIP7 CVs (the `experiment_id` [Global Attribute](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Global_Attributes/) for each MIP experiment)

Registration links for all the above, as well as other guidance for CMIP7 Community MIPs, can be [found here](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Guidance_for_MIPs/).

This document explains how a data request can be created as part of the Unharmonised DR by using the tools in this git repository, as well as the [DR Software](https://github.com/CMIP-Data-Request/CMIP7_DReq_Software).

### What this repository does:

- Collect new data requests in a central place, making them easily available to modelling centres, other MIPs, and data users;
- Store data requests in a consistent format, to facilitate use by software tools;
- Facilitate technical checks on data request (compliance with metadata standards, etc) that enable produced data to be published on ESGF. 

### What this repository does *not* do:

- Register MIPs or experiments (registration links can be found [here](https://wcrp-cmip.github.io/cmip7-guidance/docs/CMIP7/Guidance_for_MIPs/));
- Define new data request variables (aka CMOR variables), which instead is planned for the **WCRP Variable Registry** under development;
- Instigate an extensive review process of requests as occurred for the community consultation that created the Harmonised DR.

MIPs are responsible for coordinating with modelling centres to carry out MIP experiments and publish the requested variables, which may include estimating the required resources that a modelling centre must commit (such as HPC time to run the experiments and data storage capacity to publish the data).
This repository is a technical resource to make data request information easily available to them.
It does not imply coordination with modelling centres by the CMIP IPO on behalf of MIPs.


## How to create a data request

A new data request is created by submitting a pull request in this repository. The basic procedure is:

1. Clone this repo and checkout a new branch
2. Define a new Opportunity, and any new Experiment or Variable Groups that the Opportunity references
3. Run the validation script to check technical compliance
4. Submit pull request for review

See below for a detailed explanation of each of these steps. 


### 1. Clone the repository

Clone the git repository and checkout a new branch:
```bash
git clone git@github.com:CMIP-Data-Request/CMIP7_DReq_Unharmonised.git
cd CMIP7_DReq_Unharmonised/
git checkout -b name_of_mip
```
The branch name is arbitrary, but it is suggested to use the MIP's name (replacing `name_of_mip`) to help keep track of different MIPs' pull requests.


### 2. Define an Opportunity, Experiment Groups, and Variable Groups

An "Opportunity" in the DR lists the variables requested from a set of experiments and explains why they're requested.
Creating a new Opportunity is how a new request is added to the Unharmonised DR.

The experiments and variables requested by an Opportunity are specified by referring to Experiment Groups and Variable Groups.
These can be new groups created in the Unharmonised DR repo, or existing groups from the Harmonised DR (for example, `DECK` or `baseline_monthly`).

The `yaml` files specifying new Opportunities, Experiment Groups, or Variable Groups can be created in any order (all of the files will be submitted together in a single pull request in [Step 4](#4-submit-pull-request-for-review)).
The following subsections explain how to create them.


#### Creating a new Opportunity

Start by copying the Opportunity template that is provided:
```bash
cd Opportunity/
cp TEMPLATE_Opportunity.yaml mip_request.yaml
```
replacing `mip_request.yaml` with a descriptive file name; for examples from the Harmonised DR, look in the `reference_Harmonised/Opportunity` folder.
The Opportunity file should remain in the `Opportunity` folder.

Edit the file to describe the new Opportunity.
The Opportunity description can be brief or detailed.
For examples from the Harmonised DR, look at some of the `yaml` files in `reference_Harmonised/Opportunity`.
Some Harmonised DR Opportunities go into considerable detail, but this is not mandatory for the Unharmonised DR.
It is valuable include links to reference information, such as a MIP documentation paper, if possible.

The `yaml` files in `reference_Harmonised/Opportunity` are provided so that examples of Harmonised content are easily available in the same format as requested for the Unharmonised content.
Harmonised DR content can also be viewed using:

- [Airtable](https://bit.ly/CMIP7-DReq-latest)
- [DR web viewer](https://cmip-data-request.github.io/cmip7-dreq-webview/latest)

The choice of viewer is solely a user preference; the content is the same. 
(Note however there is currently no plan to make Unharmonised content available at the above viewers.)


#### Creating a new Experiment Group

Experiment Groups listed in the Opportunity file can be:

- from the Harmonised DR, as listed in the `reference_Harmonised/Experiment_Group` folder
- from other MIPs, as listed in the `Experiment_Group` folder
- brand new

To create a new Experiment Group, follow the example file (`Experiment_Group/example_experiment_group.yaml`) and reference the new group from the Opportunity file.
The new group's `yaml` file should remain in the `Experiment_Group` folder.


#### Creating a new Variable Group

Similarly to new Experiment Groups, Variable Groups listed in the Opportunity file can be existing ones (from either the Harmonised or Unharmonised DR) or new ones (following the example: `Variable_Group/example_variable_group.yaml`).
The new group's `yaml` file should remain in the `Variable_Group` folder.

Variables in a Variable Group `yaml` file should be named by their CMIP7 Compound Name (example: "atmos.uas.tavg-h10m-hxy-u.day.glb"), as shown in the `yaml` files in `reference_Harmonised/Variable_Group`.

A priority level (High, Medium or Low) should be specified for each Variable Group in its `yaml` file.

For further help in creating variable groups, [see here for some tools](#help-with-variable-groups).


### 3. Run the validation script

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


### 4. Submit pull request for review

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

Since the whole collection of variables in the Harmonised DR is quite large (roughly 2000 variables), some guidance is provided here on how this collection can be searched to find relevant variables for inclusion in a new Variable Group.

#### DR webviewer variable search

The [DR webviewer variable search](https://cmip-data-request.github.io/cmip7-dreq-webview/variable_search.html) can be used to search for variables of interest.
They can then be listed in a Variable Group `yaml` file by their CMIP7 Compound Names (example: "atmos.uas.tavg-h10m-hxy-u.day.glb").

#### Airtable filters and spreadsheet download

The Variables table in the [DR Airtable](https://bit.ly/CMIP7-DReq-latest) can be searched using Airtable's filtering/viewing options to isolate variables of interest. 
Variable Groups already defined in the Harmonised DR can also be viewed in the [DR Airtable](https://bit.ly/CMIP7-DReq-latest) as well as the [DR webviewer](https://cmip-data-request.github.io/cmip7-dreq-webview/latest/variable_groups.html).

A spreadsheet (`csv` format) listing of DR variables and their metadata attributes can be downloaded from Airtable.
In the Variables table, go to **MASTER** (near top left), open its drop-down menu, and click "Download CSV". 


#### DR software

The DR software has a command-line tool to produce a listing of DR variables and their metadata attributes in `json` or `csv` (spreadsheet) format.
[See here](https://github.com/CMIP-Data-Request/CMIP7_DReq_Software) for instructions on how to install the DR software.

To produce a `json` file listing all DR variables:
```bash
get_variables_metadata v1.2.2.5 vars.json
```
To produce the equivalent information in a spreadsheet format, the same command is used but with a filename with `csv` extension  specified as the output:
```bash
get_variables_metadata v1.2.2.5 vars.csv
```

By default, these output files include the roughly 2000 variables defined in the Harmonised DR. 
To isolate only variables of interest, the list can be filtered:
```bash
get_variables_metadata v1.2.2.5 vars.csv -f filter_variables.yaml
```
where `filter_variables.yaml` is a `yaml` file that specifies how to subset the original (large) list of variables.
For example, if the contents of `filter_variables.yaml` are:
```yaml
frequency:
- day
- 3hr
variableRootDD:
- uas
- vas
```
then only `uas` and `vas` at both `day` and `3hr` frequencies are retained.

A produced `json` or `csv` file listing DR variables may be useful for submitters to examine the metadata attributes of variables and decide which ones to include.
Note, however, that the inclusion of metadata in the `json` or `csv` file is *for information only*, to make it easier to select relevant variables. 
The metadata cannot be edited to change the definition of a variable (for example, its cell methods, dimensions, or CF standard name). Defining *new* variables as part of the Unharmonised DR is *in development* and not yet available.

If a produced `json` or `csv` file is edited to retain only the variables that are wanted in a new Variable Group, for convenience another command-line tool in the DR Software is available to convert the edited file into a `yaml` for the Variable Group (i.e., a `yaml` file that can be added to the `Variable_Group` folder and submitted as part of the pull request in [Step 4](#4-submit-pull-request-for-review)).
This is done by running:
```bash
make_variable_group vars.csv my_new_variable_group.yaml
```
where the output file should be some sensible descriptive name (see the `reference_Harmonised/Variable_Group` folder for examples).
This produces a template `yaml` file with the names of variables listed, and also validates the input against DR reference content.

⚠️ If a `csv` file is used as the input, the file that is input to `make_variable_group` **must have the same format** as the original `csv` file: the first row gives the column headers, and subsequent rows list the variables.

If `make_variable_group` is used, users should still edit the resulting `yaml` file (`my_new_variable_group.yaml` in the above example) to fill in the descriptive fields.
This includes specifying a priority level (High, Medium or Low) for the Variable Group.


## FAQ

> How long should this process take?

Assuming that a request has passed the automated technical checks (Step 3) and been approved by MIP leads, review of the pull request (Step 4) by the WIP-Ops (WIP Operations) team should be feasible quickly, within roughly a week (a more refined estimate should be possible once we have some experience with the process).
Prior to reaching this stage, it is up to a MIP to determine how much time is needed to draft its data request.
The MIP might wish to consult with its community prior to submitting, or mark its pull request as "draft" and invite community comments prior to marking the pull request as MIP-approved and ready for WIP-Ops review (Step 4).

> Will data produced from these requests pass ESGF QC (Quality Control) checks? 

Technically compliant requests should enable modelling centres to produce data that will pass QC checks and hence be publishable as CMIP7 data on ESGF.
Here "technically compliant" means that the requested experiments and variables are defined in the CMIP7 CVs. 

> If the workflow doesn't work, what do I do?

Please help us improve the tools by [opening an issue](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/issues) describing the problem.

> Do I need to be a git expert to use this?

Some familiarity with git and github is assumed. 
We have tried to make the tools as simple as possible while still allowing for automated checks of technical validity.
The technical checks are important to ensure that requested variables and experiments confirm to CMIP7 data standards, making them publishable on ESGF.
Constructive feedback to help improve the process is welcome: you can [open an issue](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/issues) or can [start a dicussion](https://github.com/CMIP-Data-Request/CMIP7_DReq_Unharmonised/discussions).

> Why is Unharmonised content stored as `yaml` files?

`yaml` files are easily readable by both humans and software.

> I don't like `yaml`, can I use spreadsheets to make a data request?

Yes, provided the information is converted into `yaml` format for the pull request. 
[See here](#help-with-variable-groups) for guidance on using the DR software to do this.

> Who reviews these pull requests?

MIP requests should be submitted with the approval of MIP leads (who must have registered their contact info with the CMIP IPO).
Pull requests will be reviewed for technical compliance by the WIP-Ops (WIP Operations) team, who maintain this repository.
MIP leads are responsible for the scientific validity of the request.

> Why do experiments and variables need to be listed in Experiment Groups and Variable Groups?

Grouping them together makes data requests easier to read and understand, and encourages re-use of existing groups where possible (including groups from the Harmonised DR).

> Can I request changes to Harmonised DR content by making a pull request in this repo?

No. The CMIP7 Harmonised DR is focused on Fast Track experiments and is finalised (subject to minor updates when errors are found). 
Find the latest version of the Harmonised DR [here](https://wcrp-cmip.org/cmip7-data-request-latest).

> How does this process differ from development of the Harmonised DR?

The Harmonised DR is targetted at the CMIP7 AFT and was developed in an extensive community consultation and review process, as described in the[Data request overview paper in CMIP7 GMD special issue](https://doi.org/10.5194/gmd-19-8053-2026) and on the [CMIP7 Data Request website](https://wcrp-cmip.org/cmip-phases/cmip7/cmip7-data-request/).
The Unharmonised DR has no such development process.
Scientific review is the responsibility of contributing MIPs, and technical review is aimed only at ensuring experiments and variables listed in a request are compliant with CMIP7 data standards so that they are publishable on ESGF.

> Is Unharmonised DR content versioned as part of the Harmonised DR versioning?

No, they are versioned separately.
This reflects their different development approaches, and also the fact that the Harmonised DR is now stable pending minor technical updates only when deemed necessary.
The Unharmonised DR, in contrast, is expected to evolve significantly, determined by MIP constributions on timelines unconstrained by the AFT.

> Does submitting a request here mean that it will become part of the CMIP8 data request?

Not necessarily, but it could improve its chances. 
While focused on CMIP7, the Unharmonised DR is also intended to smooth the path toward incorporating community MIP requests into future iterations of the Harmonised CMIP DR.

