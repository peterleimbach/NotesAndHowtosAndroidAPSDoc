# How to add a new language to AndroidAPS

## The logic behind

The markdown (rst) files which are translated via Crowdin are already available in GitHub in the folder docs/CROWDIN/**`<lang_code>`**.

"Read the Docs" (aka RTD) must now be able to build the html-files out of theses files und serve them to the readers on request.

For every language an own RTD project has to be created to enable the easy switching with the langugae switcher when you are reading the AndroidAPSDocs.

Therefore in step 2 we make the project known to RTD.

RTD needs a config file docs/CROWDIN/**`<lang_code>`**/conf.py which has to be created by copying this file from another **`<lang_code>`**. This is not translated in Crowdin as it is technical configuration.

Therefore in step 1 we copy the conf.py file into the new **`<lang_code>`** folder.
We change the order because it is better to be prepared when definig the project in RTD as we then can directly run the Build manually in RTD to check if the configuration file is right before we start it remotely via the Github action.

As the build for the RTD is not done manually we need to adapt three action scripts in Github too.

These are
1. Link Checker,
2. Build Warnings and 
3. finally the Trigger RTD Build which start the build on the RTD side.

This is done in the step 3.

And last but not least we have to change the Readme.md to add a row to 

## the concrete steps to perfrom

### Add configuration file for translation: (open a PR)
Copy https://github.com/openaps/AndroidAPSdocs/blob/master/docs/CROWDIN/de/conf.py
To https://github.com/openaps/AndroidAPSdocs/blob/master/docs/CROWDIN/**`<lang_code>`**/conf.py

It's a very small file but necessary.

```
import os

exec (open("../../shared.conf.py").read())
```

### Define the project in RTD

1. In project from https://readthedocs.org/dashboard/ "Import Project", "Import Mannualy"

2. https://readthedocs.org/dashboard/import/manual/?

    Name: AndroidAPS-**`<lang_code>`**

    Repo URL: https://github.com/openaps/AndroidAPSdocs 

    Language: <Lang>

    Advanced Settings: Python configuration file:  docs/CROWDIN/**`<lang_code>`**/conf.py

### Link the main site to the translation site in RTD

1. https://readthedocs.org/dashboard/androidaps/translations/

2. Choose which project you would like to add as a translation. Project: AndroidAPS-**`<lang_code>`**


### Automate the build process in GitHub

#### Build Warnings
1. Add PR to  https://github.com/openaps/AndroidAPSdocs/blob/master/.github/workflows/build-warnings.yml

    The **`<lang_code>`** must be added to the language array in row 17 **and** 65.

```
    language: [en, fr, de, nl, cs, el, es, ko, lt, ru]

```

#### Link Checker
1. Add PR to  https://github.com/openaps/AndroidAPSdocs/blob/master/.github/workflows/link-checker.yml

    The **`<lang_code>`** must be added to the language array in row 18 **and** 73.

#### Trigger RTD Build 
Add a trigger to build files  (Info https://docs.readthedocs.io/en/stable/webhooks.html#github )

1. Add integration https://readthedocs.org/dashboard/androidaps-**`<lang_code>`** /integrations/

    1. Integration type: Generic API incoming webhook

    2. copy the Address and  Token (for step 2 and 3)

2. Create GitHub Secrets see: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository

    1. use name RTD_WEBHOOK_KEY_**`<lang_code>`** value 

3. Add PR to  https://github.com/openaps/AndroidAPSdocs/blob/master/.github/workflows/trigger-rtd-build.yml

    1. Copy section https://github.com/openaps/AndroidAPSdocs/blob/master/.github/workflows/trigger-rtd-build.yml#L50-L53

    2. Update use the token name create above. secrets.RTD_WEBHOOK_KEY_**`<lang_code>`**

    3. Update URL section https://readthedocs.org/api/v2/webhook/androidaps-**`<lang_code>`**/<instance_id>/
    
#### Update Readme.md
In the Readme.md file a new row for  **`<lang_code>`** is to be added to the Doc Status table.
