---
layout: post
title: Using Entity Framework 6 with Database-First approach for SQLite Database in Visual Studio 2019
author: Nghia Nguyen
tags:   [EF6, SQLite]
comments_id: 3
---
Using Entity Framework 6 Designer with Database-First approach (creating EDMX file) for SQLite Database in Visual Studio 2019 is a bit tricky. Below are steps you need to follow to be successful.

The key point to be successful is to use the same version of __System.Data.SQLite__ in all steps. I use __1.0.114.x__ here at the time I am writing this post.

## Step 1. Install __System.Data.SQLite__ package

- Download [sqlite-netFx46-setup-bundle-x86-2015-1.0.114.0.exe](https://system.data.sqlite.org/downloads/1.0.114.0/sqlite-netFx46-setup-bundle-x86-2015-1.0.114.0.exe) from [System.Data.SQLite Downloads](https://system.data.sqlite.org/index.html/doc/trunk/www/downloads.wiki) (notes __1.0.114.0__ at the end of file name, it is version of __System.Data.SQLite__)

    In case you need to use different version than mine, you need to select a package with the note: "__This is the only setup package that is capable of installing the design-time components for Visual Studio 2015.__". If not, you will not be able to create model from SQLite database using EF6 Designer.

    ![](/asset/images/2021-06-10/Download System.Data.SQLite package version.png)

- Run the downloaded file `sqlite-netFx46-setup-bundle-x86-2015-1.0.114.0.exe` as Administrator to setup, select __Full installation__ and check all checkboxes in the __Select Additional Tasks__ dialog.

    ![](/asset/images/2021-06-10/Setup - System.Data.SQLite - Full installation.png)
    ![](/asset/images/2021-06-10/Setup - System.Data.SQLite - Select Additional Tasks.png)


## Step 2. Install __SQLite and SQL Server Compact Toolbox__ Extension to Visual Studio 2019

- Open Visual Studio 2019, search and insall the extension named __SQLite and SQL Server Compact Toolbox__. I used version 4.7.691. Restart Visual Studio to apply changes.

    ![](/asset/images/2021-06-10/Install VS Extension - SQLite and SQL Server Compact Toolbox.png)

## Step 3. Create a new project

- Open Visual Studio 2019, create a new Console Application project with Target framework = .NET Framework 4.6.2

    ![](/asset/images/2021-06-10/VS - Create new project .NET Framework 4.6.2.png)

## Step 4. Install NuGet Packages

- In the newly created project, open the NuGet Package Manager window for the project

- Search and install __EntityFramework 6.4.4__

    ![](/asset/images/2021-06-10/VS NuGet package - EntityFramwork 6.4.4.png)

- Search and install __System.Data.SQLite 1.0.114.3__ (I should have used the same version of __System.Data.SQLite__ that I used in __Step 1__, but I cannot find out that one, so I used __1.0.114.3__ instead, and it worked because it is just a different build, I think)

    ![](/asset/images/2021-06-10/VS NuGet package - System.Data.SQLite 1.0.114.3.png)

## Step 5. Connect to SQLite database

- Copy sample __SQLiteDB.db__ file from [here](https://github.com/nghianguyen09/ef6-sqlite/SQLiteDB.db) (or you can create your own one from <https://sqliteonline.com>) to the project folder

- In Visual Studio 2019, click __Tools__ -> __SQLite/SQL Server Compact Toolbox__ to open SQLite/SQL Server Compact Toolbox window

- In the SQLite/SQL Server Compact Toolbox window, (1) select __Add SQLite Connection...__, (2) browse or enter the path pointing to the __SQLiteDB.db__ file you just copied to your project folder, (3) click __Test Connection__ button. You should see message of "Test succeeded!". (4) hit __OK__ and (5) __Close__ to save the changes.

    ![](/asset/images/2021-06-10/Add SQLite connection.png)

## Step 6. Create EDMX file

- In Solution Explorer window of Visual Studio, select __Add__ -> __New Item...__ -> __ADO.NET Entity Data Model__, enter the name for the Model and click __Add__ button.

    ![](/asset/images/2021-06-10/Add New Item - ADO.NET Entity Data Model.png)

- In the *Choose Model Contents* step of __Entity Data Model Wizard__, select __EF Designer from database__

    ![](/asset/images/2021-06-10/Entity Data Model Wizard - Choose Model Contents.png)

- In the *Choose Your Data Connection* step, (1) select the __New Connection...__ button.  Make sure that the __SQLite Provider__ is selected, if not, click the __Change__ button to select the correct one. (2) enter the correct path to the __SQLiteDB.db__ file in your project folder. (3) click the __Test Connection__ button, you should see the message of "Test connection succeeded." (4) click the __OK__ button to close the message window. (5) click the __OK__ button.

    ![](/asset/images/2021-06-10/Entity Data Model Wizard - Choose Your Data Connection.png)

- Check the _Save connection settings in App.Config as_ checkbox, enter the name and click the __Next__ button.

    ![](/asset/images/2021-06-10/Entity Data Model Wizard - Choose Your Data Connection - Next.png)

- In the *Choose Your Database Objects and Settings* step, select __Tables__ that you want do model, enter __Model Namespace__, then click the __Finish__ button.

    ![](/asset/images/2021-06-10/Entity Data Model Wizard - Choose Your Database Objects and Settings.png)

    Click __OK__ if you are asked something about Security Warning like below (maybe 02 times)


    ![](/asset/images/2021-06-10/Security Warning.png)

- Here is the result in EDMX file

    ![](/asset/images/2021-06-10/EDMX file.png)

## Step 7. Create code to query data from SQLiteDB.db using Entity Framework

Create simple code lines to query data from the sample SQLite database file (contains 02 simple tables, __Department__ and __User__, with *Department* one-to-many *User* relationship) using SQLiteDBEntities we just created in the above steps. This is to verify the connection to SQLite database using EF6

```csharp
using System;

namespace EF6_SQLite
{
    class Program
    {
        static void Main(string[] args)
        {
            var db = new SQLiteDBEntities();

            foreach(var dept in db.Departments)
            {
                Console.WriteLine($"DeptId: {dept.DeptId}, DeptName: {dept.DeptId}");

                foreach(var user in dept.Users)
                {
                    Console.WriteLine($"         UserId: {user.UserId}, UserName: {user.UserName}, FirstName: {user.FirstName}, LastName: {user.LastName}");
                }
            }

            Console.ReadLine();
        }
    }
}
```

Start the project. Oops... the error happens
> **System.InvalidOperationException:** 'No Entity Framework provider found for the ADO.NET provider with invariant name 'System.Data.SQLite'. Make sure the provider is registered in the 'entityFramework' section of the application config file. See http://go.microsoft.com/fwlink/?LinkId=260882 for more information.'

![](/asset/images/2021-06-10/Error.png)

## Step 8. Fix error

Open the `App.config` file, this is the original `App.config` file created/updated by EF6. Add the below line to the `entityFramework\providers` section

```xml
<provider invariantName="System.Data.SQLite" type="System.Data.SQLite.EF6.SQLiteProviderServices, System.Data.SQLite.EF6" />
```

The `App.config` file __BEFORE__ changes
![](/asset/images/2021-06-10/AppConfig-BeforeChanges.png)

The `App.config` file __AFTER__ changes
![](/asset/images/2021-06-10/AppConfig-AfterChanges.png)


Start the project again. Here we go. It is working now.
![](/asset/images/2021-06-10/WorkingOutput.png)


Download sample source code from [GitHub](https://github.com/nghianguyen09/ef6-sqlite)

### References
*   <https://stackoverflow.com/questions/58767771/cant-get-sqlite-to-work-in-vs2019-with-ef6>
*   <https://stackoverflow.com/questions/29129796/no-entity-framework-provider-found-for-ef-6-and-sqlite-1-0-96-0>

Hope it helps.