# Django TODO App
#### Video Demo:  [URL HERE]
#### Description:

This Django-based web application serves as a simple yet efficient to-do list manager. Users can register an account, log in, add tasks, edit tasks, mark tasks as completed, and view all their tasks in a neat list format.

### File Structure and Descriptions:

- `base` (Directory): 
  - This is the main application directory containing all the required Django modules and templates.
    - `__pycache__`: Contains Python bytecode compiled files which help in faster execution.
    - `migrations`: Contains database schema migrations files used by Django ORM.
    - `templates/base`: 
      - Houses the HTML templates for various pages and functionalities of the application.
        - `login.html`: Template for the login page.
        - `main.html`: The primary template for the homepage after a user logs in.
        - `register.html`: Template for the user registration page.
        - `task_confirm_delete.html`: Confirmation page for deleting a task.
        - `task_form.html`: Form template to add or edit tasks.
        - `task_list.html`: Displays the list of tasks added by the user.
        - `task.html`: Template for individual tasks with details.
    - `__init__.py`: This file is required for Python to recognize this directory as a package.
    - `admin.py`: Contains configurations for Django's built-in admin interface.
    - `apps.py`: Application configuration.
    - `models.py`: Contains database models for the application.
    - `tests.py`: A file to write tests for the application functionalities.
    - `urls.py`: Contains URL patterns for routing.
    - `views.py`: Contains views (controller logic) for handling requests and rendering responses.

- `todo` (Directory): 
  - This seems like the main project directory.
    - `db.sqlite3`: SQLite database file used by Django by default.
    - `manage.py`: Command-line utility for administrative tasks.
    - `requirements.txt`: Lists all the Python packages required for the project.
    - `vercel.json`: Configuration file if deploying with Vercel.

### Design Choices:

Throughout the development of this application, certain design choices were made to ensure simplicity, scalability, and usability. Using Django's built-in user authentication system simplified the process of user registration and login. The SQLite database was chosen for this application because of its ease of use and integration with Django, especially for smaller applications. 

The `base` application structure was maintained to keep all core functionalities in one place, making it easier to manage and expand in the future. The HTML templates were designed with a user-friendly interface in mind, allowing users to easily interact with their tasks.


### views.py
This file contains the core views for the to-do application, providing functionalities for user authentication and CRUD operations for tasks.

#### CustomLoginView
- A custom login view based on Django's built-in `LoginView`.
- Uses `base/login.html` as its template.
- Redirects authenticated users to the tasks page.

#### RegisterPage
- A registration view for creating new user accounts.
- Uses `base/register.html` for its template.
- Redirects authenticated users to the tasks page.

#### TaskList
- Displays all tasks specific to the authenticated user.
- Implements search functionality based on task titles.

#### TaskDetail
- Displays details of a specific task.
- Uses `base/task.html` as its template.

#### TaskCreate
- Provides a form to create new tasks.
- Associates new tasks with the logged-in user.

#### TaskUpdate
- Allows users to update the details of an existing task.

#### TaskDelete
- Provides the functionality to delete a task.

---

These views utilize Django's generic views and mixins for efficient development. The `LoginRequiredMixin` ensures that a user must be logged in to access specific views.

## URL Configuration (`urls.py`)

The `urls.py` file is vital in Django as it directs incoming web requests to the appropriate view based on the URL. Here's the detailed breakdown of each URL pattern:

### User Authentication URLs:
- **Login:** `login/`
  - Directs to `CustomLoginView`.
  - Used for user login.

- **Logout:** `logout/`
  - Utilizes Django's built-in `LogoutView` and redirects to `Login` page upon successful logout.

- **Register:** `register/`
  - Directs to `RegisterPage`.
  - Handles user registration.

### Task Management URLs:
- **Tasks List:** `/`
  - Maps to `TaskList` view.
  - Displays the list of tasks for the authenticated user.

- **Task Detail:** `task/<int:pk>`
  - Maps to `TaskDetail` view.
  - Displays detailed information of a specific task.

- **Create Task:** `task-create/`
  - Directs to `TaskCreate` view.
  - Allows users to create a new task.

- **Update Task:** `task-update/<int:pk>`
  - Maps to `TaskUpdate` view.
  - Lets users update an existing task.

- **Delete Task:** `task-delete/<int:pk>`
  - Directs to `TaskDelete` view.
  - Users can delete a task.

Each URL pattern is associated with a specific view and has a unique name that can be referred to in Django templates and views for reverse URL matching.

## Testing (`tests.py`)

The `tests.py` file is designated for writing unit tests to validate the functionality of the application. Proper testing ensures that the application behaves as expected and is free from defects.

Currently, no tests have been defined in this file. However, Django's built-in `TestCase` class has been imported, which means unit tests can be added in the future using this class.

## Data Models (`models.py`)

The `models.py` file in Django is responsible for defining the data structures of the application. These structures, referred to as models, represent the tables in the database. Here's the detailed breakdown of the models:

### Task Model:
- **user:** 
  - A foreign key relationship to the `User` model provided by Django's authentication system.
  - Represents the user to whom the task belongs.
  - The `on_delete=models.CASCADE` argument ensures that if a user is deleted, all of their associated tasks will be deleted as well.
  - Fields can be left blank or set to null.

- **title:** 
  - A character field with a maximum length of 200 characters.
  - Represents the title or name of the task.

- **desc:** 
  - A text field to capture a more detailed description of the task.
  - This field is optional and can be left blank or set to null.

- **complete:** 
  - A boolean field that indicates whether the task is completed or not.
  - By default, when a task is created, it's set to `False` (not completed).

- **created:** 
  - A date-time field that automatically captures the date and time when a task is created.

The `__str__` method returns the title of the task, which provides a human-readable representation of the model.

Additionally, within the `Meta` class, the `ordering` attribute ensures that tasks are ordered based on their completion status in the database.

## Application Configuration (`apps.py`)

The `apps.py` file is used to configure specific settings for the Django app. This file contains details about the app's configuration which can be helpful during the initialization of the app or when integrating it with other Django projects.

### BaseConfig Class:

- **default_auto_field:** 
  - Set to `'django.db.models.BigAutoField'`.
  - This configuration defines the type of auto-incrementing primary key to use by default for models that don't specify a primary key explicitly.
  - The `BigAutoField` type uses a 64-bit integer to store its values, allowing for a larger number of unique keys compared to the default `AutoField`.

- **name:** 
  - Set to `'base'`.
  - It indicates the name of this app, which is essential for Django's internal mechanisms when referring to the app.

This configuration ensures that the application uses consistent settings and can be crucial for its smooth operation within the Django project.

## Admin Configuration (`admin.py`)

The `admin.py` file is a part of Django's built-in admin site. It provides a way to define how your models are displayed and interacted with on the admin interface. By registering models with the admin site, you can easily manage them using Django's out-of-the-box administrative capabilities.

### Model Registration:

- **Task Model:**
  - The `Task` model, which is defined in the `models.py` file, has been registered with the admin site.
  - By registering the `Task` model, it becomes accessible and manageable through the Django admin interface.

Utilizing the Django admin interface provides a quick and straightforward way to manage data without the need for an external tool or writing additional code. This makes data management more streamlined and efficient for developers and administrators alike.

## Main Layout (`main.html`)

The `main.html` file serves as the base template for your To-Do application. It defines the common structure of the website, including the navigation bar and footer, while allowing for dynamic content injection.

### Main Sections:

1. **Meta Information**:
   - Specifies the document type (`DOCTYPE html`).
   - Defines character set as `UTF-8`.
   - Sets the viewport to scale properly on all devices.

2. **Links & Scripts**:
   - **Bootstrap CSS**: Provides styling and responsiveness.
   - **jQuery**: JavaScript library for interactivity.
   - **Popper JS**: Used for popovers in Bootstrap.
   - **Bootstrap JS**: Provides interactivity for Bootstrap components.
   - **Google Fonts**: Imports 'Fira Sans Extra Condensed' font.

3. **Styles**:
   - Global styles for the body, container, and various other elements.
   - Navbar styles and transitions.
   - Styles for footer elements.

4. **Body**:
   - **Navbar**: Contains the site's logo ("Just Do It!") and mobile responsiveness toggler.
   - **Main Content Container**: A designated space where the content from other templates will be injected using Django's template inheritance (`{% block content %}`).
   - **Footer**: Contains copyright information.

The `main.html` template ensures a consistent look and feel across all pages, while also allowing for flexibility where specific content can be injected. This way, you can maintain a cohesive design while still customizing the content for different views.

## Login Template (`login.html`)

The `login.html` template provides the structure for the login page of your application. It extends from the `main.html` template to ensure consistency in design across all pages.

### Structure:

1. **Extension from Main Template**:
   - Inherits the base structure from `main.html`.

2. **Main Content**:
   - A centered `row` which will be placed in the main content container inherited from `main.html`.
   - Within the row, a column of width 6 (out of 12) is defined to house the login card.
   
3. **Login Card**:
   - **Card Header**:
     - Background color is set to primary.
     - Contains the text "Login" styled with a white font color.
   - **Card Body**:
     - Contains a form for login.
     - The form uses the `POST` method.
     - Django's `csrf_token` is included for security.
     - The login form is rendered using Django's form methods (`{{ form.as_p }}`), which means each form field is wrapped in a paragraph tag (`<p>`).
     - A submit button is present with the value "Login".
   - **Card Footer**:
     - Provides a link for users who don't have an account, redirecting them to the registration page (`Register`).

### Styling & Responsiveness:

- The `login.html` template uses Bootstrap classes for styling and responsiveness.
- The `card` component provides a neat and organized look for the login form.
- The template ensures that the login card will be centered on the page, thanks to the `justify-content-center` class.

---


