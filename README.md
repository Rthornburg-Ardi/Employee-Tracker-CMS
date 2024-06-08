# Employee Database Content Management System (CMS)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## Description

Data management is an essential service necessary for companies across many industries. Business leaders often need a way to view and manage data about their organization without crafting time-intensive SQL queries themselves. Given the dynamic nature of workplaces, whether a company is scaling up from a small start-up, adjusting to COVID-19, or restructuring to better meet an industry-specific goal, it is vital for a company to be able to accurately manage and track its organizational structure.  This application provides the functionality for a manager or executive to view and modify data on company departments, job roles, managers, and employees. Acting as a Content Management System, the application provides a smooth interface to display and update data in the database while sparing the user from working directly with the intricacies of SQL queries.

The application is run from the command line, and it prompts the user with options for viewing, adding, updating, or deleting various elements of the organizational chart. After the user chooses an option, a labeled table or response message is printed to the terminal in a clean, visually appealing format. The user is then prompted to choose another option, allowing them to conduct multiple operations until they choose the "Quit" option. The ability to manage data on departments, job roles, and employees in this user-friendly manner can help a company to communicate and maintain its organizational structure, especially in the midst of company growth or restructuring. In turn, the degree to which a company is able to successfully articulate and sustain its structure can help potential investors decide whether to back the company. Content Management Systems provide a high value to non-technical individuals who need to work with databases, and this Employee Database CMS is an apt solution for accurately managing data on a company's structure.

## Table of Contents

[Installation](#installation)

[Usage and Features](#usage-and-features)

[Contributing](#contributing)

[Credits](#credits)

[Questions](#questions)

[License](#license)

## Installation

PostgreSQL is the database management system that allows this application to provide the results of PostgreSQL queries. Download PostgreSQL [here](https://www.postgresql.org/download/).

This application uses npm package node-key-sender to offer a "Quit" menu option which works across operating systems. The node-key-sender package requires Java, which can be downloaded at [https://www.java.com/en/](https://www.java.com/en/). If you don't already have Java and prefer not to download it, `Command + .` can be used to quit the terminal process for MAC Keyboards, and `Control + C` quits the process for Windows Keyboards. 

To run the application, first clone the repository and navigate to the root of the depository in your terminal. Then, run following commands, in order, in your terminal:

* `npm install`
* `psql -U postgres`
    * enter following password when prompted: aloesonarcat
    * `\i db/schema.sql;`
    * `Control + C` (Windows) or `Command + .` (MAC) (exits the postgreSQL console)
* `npm run seed`
* `npm run start` 

The `npm install` command will install the application's dependencies. `\i db/schema.sql;` will create the database. `npm run seed` will populate the database with default data, and `npm run start` will run the application.

## Usage and Features

The following descriptions use the seed data in seeds.sql to show the functionality of the application. Feel free to follow along with the screenshots or the walkthrough video accessible [here](https://drive.google.com/file/d/1FK4Gv780pCeD9os647CAIzc9D0IdaIAQ/view?usp=sharing).

After typing npm run start in your terminal, you will be presented with the following menu of options. Navigate through the options using the arrow keys and hit enter when your desired option is selected.

![screenshot1](/assets/screenshot1.png)

When selecting the "View all departments" option, the following table is printed to the terminal. The department with the id of 6, Unspecified Department, is the default department for a job_role when its previous department has been deleted. 

![screenshot2](/assets/screenshot2.png)

The "View all roles" option produces a table with the id, title, salary, and department name for each role. The role with id = 31, "Unspecified Role", is the default role for an employee if the employee's previous role is deleted.

![screenshot3](//assets/screenshot3.png)
![screenshot4](/assets/screenshot4.png)

After selecting the "View all employees" option, a table is printed to the console which displays each employee's id, first and last name, title, department name, and salary, and the first and last name of the manager the employee reports to.

![screenshot5](/assets/screenshot5.png)
![screenshot6](/assets/screenshot6.png)

To view all of the employees under one manager, select the "View employees by manager" option. Per the seed data, the employees working under Maya Kapoor are James Johnson, Sophia Williams, Amir Khan, Olivia Brown, and Michael Jones. 

![screenshot7](/assets/screenshot7.png)

The schema and relationships designated for employees and managers dictate that all managers are included in the employee table as employees themselves. To facilitate the relationship between managers and their employees, managers have a value of null for `manager_id`, and employees reporting to a manager have a `manager_id` which references the employee id of their manager. Just as it is important for a company to maintain a sense of order in the chain of command, it is important to be able to differentiate between managers and employees in the database. If the user searches for employees under an employee who is not a manager, an informative error will be thrown, stopping the execution of the program.

![screenshot8](/assets/screenshot8.png)

Throughout the application, prompts requiring user input have checks for blank/missing user input and will throw errors if the necessary user input was not obtained. This helps the user to course correct and prevents issues with database queries. The below screenshot shows an error thrown in response to missing input for this prompt.

![screenshot9](/assets/screenshot9.png)

The below table shows an example of viewing employees by department—employees in the Technology department, specifically. In the same screenshot, the user entered a department which doesn't exist in the database (Customer Service) so an error was thrown.

![screenshot10](/assets/screenshot10.png)

Simply viewing all of the important data on employees in one place, or viewing the relationships between employees and managers, can be valuable. But, the functionality to modify data is essential for a dynamic, growing company. The below screenshot shows an example of adding a new department, Sales, as well as a failsafe which prevents the application from continuing with missing input.

![screenshot11](/assets/screenshot11.png)

The below images show the functionality of adding a new role. These screenshows also show an abbreviated printout of the "All Roles" table with the new addition, Director of DevOps, at the bottom of the table.

![screenshot12](/assets/screenshot12.png)
![screenshot13](/assets/screenshot13.png)

If the user attempts to add a new role in a department that does not exist, an error will prevent the addition from being completed. The below screenshot shows the first few lines of the error message.

![screenshot14](/assets/screenshot14.png)

When adding a new employee to the database, make sure to provide values for the employee's manager and role which already exist in the database. As seen in the below screenshots, Tasha Singh, a Content Marketing Manager (an existing role) reporting to Logan Sullivan (an existing manager), was able to be successfully added.

![screenshot15](/assets/screenshot15.png)
![screenshot16](/assets/screenshot16.png)

In the below example, the new employee was not added, as the role provided was not an existing role. If an HR Generalist role was needed, the user would need to choose the "Add a role" option from the menu first to add that role to the database.

![screenshot17](/assets/screenshot17.png)

The below screenshot demonstrates additional protection against inaccurate data—if an employee who is not a manager is entered as the manager for a new employee, the user will be informed that the entry is not a manager in the database currently. If, for example, James Johnson was Brett Chauncy's real world manager, this would be the user's cue to set James Johnson's `manager_id` to null. Otherwise, this custom error would cue the user to find Brett Chauncy's actual manager.

![screenshot18](/assets/screenshot18.png)

To update an employee role, the user is provided the list of employees to select from. After selecting the employee to update, the user is prompted to enter their new role. As long as the role exists, the employee will be updated. Tasha Singh's role in a previous example was Content Marketing Manager; the screenshots show that her role was successfully updated to Market Research Manager. 

![screenshot19](/assets/screenshot19.png)
![screenshot20](/assets/screenshot20.png)

If a department name changes, or a company restructures, a department can be deleted. In the below, the existing Technology department was successfully deleted. The Competitive Baking department did not exist, and could not be deleted. 

The deletion of an entire department has a sizable impact across the database, but the default department, "Unspecified Department," helps to maintain the integrity of relationships between tables. As can be seen in the next two screenshots:

* The Technology department no longer appears in the department table
* Employees with id 1-6 are now under "Unspecified Department" 
* All Technology roles now have "Unspecified Department" for their `department_name`

![screenshot21](/assets/screenshot21.png)

![screenshot22](/assets/screenshot22.png)

Employees can be deleted as well. Amir Khan had appeared in a previous table with employee id 4, and Amir was successfully removed from the database. (For good measure, Bill Gates did not exist as an employee, and was unable to be deleted.)

![screenshot23](/assets/screenshot23.png)
![screenshot24](/assets/screenshot24.png)

At any point, the user can quit the application by selecting the "Quit" option in the menu and responding Y to the "Are you sure you want to quit?" confirmation prompt. 

## Contributing

If you would like to contribute, feel free to email me at thornburg.rebeca1@gmail.com with any ideas on new features or improved functionality, create an Issue, or submit a pull request. If you create an issue, please @ me. If you would like to make a pull request, please request a pull request review from me so that I can review your proposed changes. I look forward to collaborating with you!

## Credits

I received help troubleshooting issues with asynchronous JavaScript from Nick S., and I worked on the first three postgreSQL queries with Joem Casusi. I appreciate them sharing their experience and knowledge. Many thanks for their advice and assistance!

Stack Overflow. (2015, August 21). Functions executing in wrong order. Stack Overflow. https://stackoverflow.com/questions/32136273/functions-executing-in-wrong-order

Stack Overflow. (2017, July 30). Passing values from one promise to the next. Stack Overflow. https://stackoverflow.com/questions/45401316/passing-values-from-one-promise-to-the-next

Stack Overflow. (2016, November 22). Return promise from the function. Stack Overflow. https://stackoverflow.com/questions/40735418/return-promise-from-the-function

Stack Overflow. (2021a, February 9). Returning the result of a node-postgres query. Stack Overflow. https://stackoverflow.com/questions/58254717/returning-the-result-of-a-node-postgres-query

W3 Schools. (2024). JavaScript promises. W3Schools. https://www.w3schools.com/js/js_promise.asp

## Questions

My GitHub username is Rthornburg-Ardi if you would like to connect or view my other projects. Feel free to reach out to me at https://github.com/Rthornburg-Ardi/ or thornburg.rebeca1@gmail.com if you have any further questions about this project, and I'll be glad to assist.

## License

This project is covered under the MIT License. You can learn more about this license and its coverage and permissions [here](https://opensource.org/licenses/MIT).