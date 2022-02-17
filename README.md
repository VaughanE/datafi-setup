# What is Datafi?

Datafi is new way to securely consolidate your data from where it lives and access and share it online. To learn more about Datafi, visit our [website](https://datafi.us/).

Datafi enables you to...

- access your data online
- connect data from a spectrum of sources into a single place ([full list here](#supported-datasources))
- invite others to access curated portions of your data.
- create links/references between different types of datasources.

# Quick Start

The following guide should quickly and _painlessly_ get you through the process of connecting your data to DatafiCloud with a set of easy to follow instructions.

**_( A sample dataset will be provided in [`Step 1`](#step-1-register-an-account) in case you want to jump right into exploring the console)_**

## Steps

1. [Register an Account](#step-1-register-an-account)
2. [Connect Your Data](#step-2-connect-your-data)
   - [Local Edge Server Setup (optional)](#local-edge-server-setup)
   - [Default Dataset Setup](#default-dataset-setup)
   - [Upload CSV Directly](#upload-csv-directly)
3. [Configure Dataset](#step-3-configure-your-dataset)
4. [Access Your Data](#step-4-viewing-your-data)
<!-- 5. configure policies and Access Ratings
5. invite others -->

## Step 1: Register an Account

The first thing you need do is to [**register an account**](https://dataficloud.com/register) for Datafi.

You will be asked for a name, email, and company name. You will also be provided a small selection of demo datasets to add to your workspace.

After registering, you can skip ahead to [**Step 4**](#step-4-viewing-your-data) if you just want to explore the console with sample dataset provided to you, otherwise proceed to [**Step 2**](#step-2-connect-your-data) to connect a dataset of your own.

## Step 2: Connect Your Data

There are three ways to connect your data to your Datafi workspace. You can setup a local Datafi Edge Server, use the default Datafi Edge Server, or simply upload a CSV file.

### Local Edge Server Setup

If you want to locally provision an Edge Server for your dataset, instead of using the default Edge Server run by datafi. Complete the following steps:

1. Install Docker Desktop (ref: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop))
2. Open the terminal and run the Datafi Edge Server container while opening port 80 & 443, and mapping those port to your local host with the following:

   ```
   docker run --rm -p 80:80 -p 443:443 datafi/es:latest
   ```

3. Open `http://localhost/setup`
4. Enter a name (eg: "My First Dataset") and the email you used in **Step 1**.
5. Select your datasource and input the datasource user credentials (the database user should have **_read-only_** access)
6. Submit and wait for the processing to finish, once complete a `KEY` will be displayed on screen. Copy and save it for the following step.
7. Stop the container (CTRL+C). and start again with the above generated `KEY` as an environment variable.

   ```
   docker run --rm -p 80:80 -p 443:443 -e KEY={the_above_generated_key} datafi/es:latest
   ```

8. You will receive an email with a link to connect the Edge Server and Dataset you just setup to your Datafi workspace. Follow the instructions on screen and click `Configure` and proceed to [**Step 3**](#step-3-configure-your-dataset).

- Troubleshooting: After setting up the Edge Server, if you can't see any data from the dataset, try restarting the Edge Server using the same `KEY` (repeat step 7)

### Default Dataset setup

If you would rather use the Default Datafi Edge Server, instead of provisioning your own, you can do so through the Datafi Console.

In the upper left side of the console, click the large blue Add (**+**) button and choose `Add Dataset`.

Select the type of the Dataset you want to add and enter the credentials for a user with **_read-only_** access.

Click the `Add Dataset` button in the dialog and proceed to [**Step 3**](#step-3-configure-your-dataset).

### Upload CSV Directly

If you your data exists in a **.CSV** file, you can upload it to datafi any time by clicking the large blue Add (**+**) button and choose `Upload Datafile`.

Choose the File you want to add and create a new Datafile Dataset, or add the file to an existing one. (Datafile Datasets are like normal datasets, but each file acts like a table within the overall dataset)

Click `Add Datafiles` or `Add Dataset` (depending on if you are creating a new dataset, or adding to an existing one) and once the file finishes uploading, click `Continue` and proceed to [**Step 3**](#step-3-configure-your-dataset).

## Step 3: Configure your Dataset

After Completing **Step 2** you should see an Edit Dataset dialog which will allow you to configure and edit the details for the dataset you just added. Their are four aspects of configuring a dataset, Overview, Schema, Users, and Rules. Each of which is explained below.

If you are not planning on inviting any other user to you workspace, then the Overview page is probably the only section you need to worry about and you can click `Save` and proceed to [**step 4**](#step-4-viewing-your-data)

(The dialog can be accessed again any time by clicking the edit icon of the dataset you are currently active on, in case you close it before you are finished or want to make a change.)

### Overview

In the dataset Overview you can update the name, description, and tags for your dataset.

### Schema

The Schema section allows you to configure the following:

- accessability control, determining if any specific tables or fields should be hidden/blocked.
- renaming tables and fields to have more user friendly names where necessary.
- Dataset level access rating for tables and fields can be set to have a more in depth control over who can see what information in your dataset. ([More details about Access Ratings](#access-rating-explained))

### Users

the User section allows you to invite users to access your dataset, as well as set their membership Access Ratings. You will be set as an owner for the dataset by default, which will provide you full access to the data.

### Rules

the rules sections allows you to create rules that can mask specific fields in your dataset's tables (more details on rules (WIP))

## Step 4: Viewing your data

On the [Dataview](https://dataficloud.com/dataview) page you will be able to see a list of datasource that you have access to on the left side of you screen. Choosing a table from one of them will open the contents of that dataset that your [Access Rating](#access-rating-explained) permits, and mask fields according to any rules applied to that dataset.

# References

In-depth info on various subjects pertaining to the Datafi console.

## Access Rating Explained

One of the ways that Datafi cloud enables you to secure your information is with Access Ratings. With Access Ratings you can have certain tables or fields be selectively visible to different individuals based off their Access Ratings.

Access Ratings are split into three different aspects, `Confidentiality`, `Sensitivity`, and `Identity`. Each aspect has a range of security level labels as seen below, with the values in level 0 being the least sensitive and the 5 being the most sensitive.

(Note: If you are unhappy with the default labels, they can be renamed when first setting up your workspace, or in the administration settings )

| Confidentiality | Accessability   | Identity      | Security Level |
| --------------- | --------------- | ------------- | -------------- |
| Open            | Non Sensitive   | Public        | 0              |
| Limited         | Less Sensitive  | Partners      | 1              |
| Regulated       | Sensitive       | Internal Only | 2              |
| Restricted      | More Sensitive  | Exclusive     | 3              |
| Strict          | Very Sensitive  | Secure        | 4              |
| Secret          | Hyper Sensitive | Private       | 5              |

When a user tries to view the contents of a dataset, the Access Rating for that user's membership Access Rating is compaired to the datasets Table and Field Access Rating, if all three aspects of that user's aspect rating does not meet or match the access ratings of a field, then that field is hidden.

### Setting Access Ratings

Access ratings can be set in various places, and default access ratings can also be provided.

For users, Default Access Ratings can be set at a workspace level, an individual level, and an individual user per dataset level (or membership). For Datasets Default Access Ratings are set at workspace, table, and field levels.

The Default workspace user Access Ratings and Dataset Access Ratings can be set under the `Workspace Policies` tab of the administration page.

Individual user Access Ratings can be set under the `User Managment` tab of the administration page. (defaults to the workspaces user default access rating)

User membership level Access Rating are set in the Edit Dataset dialog's `Users` tab. (defaults to user's individual user Access Ratings)

Table and field level Access Ratings are set in the `Schema` tab of the Edit Dataset dialog. (Table defaults to Workspaces default dataset Access Rating, and fields default to the table's Access Rating)

## Supported Datasources

Datafi currently supports the following datasources

- MySQL
- Salesforce
- PostgreSQL
- Maria DB
- AWS Aurora
- MSSQL
- Netsuite Cloud
- Oracle DB
- Snowflake
- Microsoft Dynamics
- CSV file

And more are in the works!
