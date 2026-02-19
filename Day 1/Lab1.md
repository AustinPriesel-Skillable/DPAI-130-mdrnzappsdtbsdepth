# Lab 01: Build a fitness app with GitHub Copilot agent mode
 test 
In this lab, participants will set up a preconfigured development
environment using GitHub Codespaces to build a full-stack OctoFit
Tracker application. They will use GitHub Copilot in agent mode to
create and publish branches, set up the backend with Django and MongoDB,
and configure the frontend using React. By the end of the lab,
participants will have an integrated, functional web application
connected to a MongoDB database, accessible and tested directly from
their GitHub Codespace environment.

## Objectives:

- Set up the development environment using GitHub Codespaces.

- Create and publish a branch with Copilot agent mode.

- Configure app structure with frontend and backend folders.

- Set up MongoDB and integrate it with Django.

- Build and test REST APIs.

- Develop and connect the React frontend.

- Apply Bootstrap styling to the app.

## Prerequisites

- A GitHub account  for using GitHub Codespaces. If you don’t have
  Github account then you can create from
  [here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home).

### Exercise 1: Set Up the Environment

1.  Sign in to your GitHub account.

    ![](./media/image1.png)

2.  Browse to the following link:
    +++https://github.com/skills/build-applications-w-copilot-agent-mode/tree/main+++

    In this lab you will create the repository using a public template
    “**skills-build-applications-w-copilot-agent-mode**”.

    ![](./media/image2.png)

3.  Select **Create a new repository** under **Use this template** menu.
    This Duplicates the lab setup under your account to make changes
    safely.

    ![](./media/image3.png)

4.  Enter the following details and select **Create Repository**.

    - Repository name:
    +++**skills-build-applications-w-copilot-agent-mode**+++

    - Repository type: **Public**

    ![](./media/image4.png)

5.  Click the green-coloured **Code** button to proceed with creating
    workspace in the cloud.

    In the drop-down pane select **Codespaces** tab. This Launches a
    cloud-based development environment with preinstalled tools.

    ![](./media/image5.png)

6.  Click **Create codespace on main** button.

    ![](./media/image6.png)

7.  Wait for some time (approximately 5 mins) for Visual Studio Code to
    load and complete creating the codespace.

    ![A screenshot of a computer AI-generated content may be
    incorrect.](./media/image7.png)

    ![](./media/image8.png)

### Exercise 2: Application Initial Setup

Now that we are familiar with the app and we know it works, let's ask
copilot for help starting a branch so we can do some customizing.

1.  Select **GitHub Copilot Chat** from the top of the vs code. This
    enables Copilot’s AI-driven coding assistance.

    ![A screenshot of a computer Description automatically
    generated](./media/image9.png)

2.  Click on **Show config** option from the copilot chat and send the
    prompt.

    ![](./media/image10.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image11.png)

    ![](./media/image12.png)

3.  Make sure ‘**Agent’** mode is selected to enable Copilot's agent
    mode. This activates automation to perform multi-step actions (like
    creating branches and installing dependencies).

    ![](./media/image13.png)

4.  Let's ask Copilot agent mode to help us remember the command and
    create the branch build-octofit-app and publishing it. The creation
    of new branch separates changes from the main branch for safer
    collaboration.

    **+++Please create and publish a new Git branch called
    build-octofit-app+++**

    Click on **Allow** button to execute the command.

    ![A screenshot of a computer Description automatically
    generated](./media/image14.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image15.png)

5.  Navigate to the repository and click on **main** **dropdown** where
    you’ll see the new branch listed as **build-octofit-app**. Open that
    branch.

    ![A screenshot of a computer Description automatically
    generated](./media/image16.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image17.png)

6.  Open up a **new Copilot Chat** session by hitting the
    plus **+ **icon in the Copilot Chat pane.

    ![A screenshot of a chat Description automatically
    generated](./media/image18.png)

7.  Write a prompt for GitHub Copilot in agent mode to start the
    creation of our application:

    ```
    Let's take the following step by step execute the commands.

    Follow the instructions

    - Follow the OctoFit Tracker App structure

    - Follow the Python virtual environment creation

    - Create the requirements.txt file

    - Install the Python requirements from the file created
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image19.png)

8.  Click on **Allow** button to execute the suggested command.

    ![A screenshot of a computer Description automatically
    generated](./media/image20.png)

9.  Click on **Allow** again to create a virtual environment.

    ![A screenshot of a computer Description automatically
    generated](./media/image21.png)

10. Again, click on **Allow** to install packages in .venv file.

    ![A screenshot of a computer Description automatically
    generated](./media/image22.png)

11. Once you complete these steps, you will get a confirmation that all
    the steps that you have mentioned in the prompt are completed.

    ![A screenshot of a computer Description automatically
    generated](./media/image23.png)

12. Navigate to **Source Control** from the left navigation menu on VS
    Code.

    ![](./media/image24.png)

13. **Commit** the change by clicking on the **Commit** button in the
    Source Control section.

    ![](./media/image25.png)

14. If you get the pop-up to stage all your changes and commit them
    directly, click on **Yes**.

    ![](./media/image26.png)

15. Click on **Sync Changes**.

    ![](./media/image27.png)

16. Click on **Yes** to give a confirmation to pull and push the changes
    from the build-octofit-app branch.

    ![A screenshot of a computer Description automatically
    generated](./media/image28.png)

17. Navigate to the GitHub repository now and you’ll receive a **Compare
    and Pull Request**. Click on it.

    ![A screenshot of a computer Description automatically
    generated](./media/image29.png)

18. Create a pull request.

    ![A screenshot of a computer Description automatically
    generated](./media/image30.png)

19. Click on **Merge Pull request.**

    ![A screenshot of a computer Description automatically
    generated](./media/image31.png)

20. Click on **Confirm merge**.

    ![A screenshot of a chat Description automatically
    generated](./media/image32.png)

21. Navigate to **Code** tab from the top and open the
    **octofit-tracker/backend** folder.

    ![A screenshot of a computer Description automatically
    generated](./media/image33.png)

22. To check the requirements, click on **requirements.txt** file.

    ![A screenshot of a computer Description automatically
    generated](./media/image34.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image35.png)

### Exercise 3: Django Project Setup

1. Open a **new chat** by clicking on the **+** icon at the top of the
    chat.

2. In GitHub Copilot Agent mode, set up the Python Django project/app.
    Then, paste the following text into the editor and click on
    **Send**:

    **+++/create-django-project+++**

    ![A screenshot of a phone Description automatically
    generated](./media/image36.png)

3. Click on **Allow** to execute the command.

    ![A screenshot of a computer Description automatically
    generated](./media/image37.png)

4. Again, click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image38.png)

5. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image39.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image40.png)

6. Paste the prompt to create octofit_db MongoDB database. This sets up
    MongoDB integration and populates the database with initial data.

    **+++/init-populate-octofit_db+++**

    ![](./media/image41.png)

7. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image42.png)

8. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image43.png)

9. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image44.png)

10. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image45.png)

11. Let’s create a prompt file that will update the Python Django
    project/app files. Copy/paste the following prompt in the GitHub
    Copilot Chat:
    ```
    Let's add the following to a prompt file called `update-octofit-tracker-app.prompt.md` in the `.github/prompts` directory and add mode: 'agent' and model: GPT-4.1 to the prompt file. 

    # Django App Updates 
    - All Django project files are in the `octofit-tracker/backend/octofit_tracker` directory. 
    1. Update `settings.py` for MongoDB connection and CORS. 
    2. Update `models.py`, `serializers.py`, `urls.py`, `views.py`, `tests.py`, and `admin.py` to support users, teams, activities, leaderboard, and workouts collections. 
    3. Ensure `/` points to the api and `api_root` is present in
    
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image46.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image47.png)

12. Let's use the prompt file to update the Python Django project/app
    files. Paste the following text on the text editor.

    **+++/update-octofit-tracker-app+++**

    ![A screenshot of a computer Description automatically
    generated](./media/image48.png)

13. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image49.png)

14. All the required files have been updated.

    ![A screenshot of a computer Description automatically
    generated](./media/image50.png)

15. On the source control tab, Add the commit message and select
    **Commit** to commit the changes.

    ![A screenshot of a computer Description automatically
    generated](./media/image51.png)

16. Click on **Yes**.

    ![A screenshot of a computer Description automatically
    generated](./media/image52.png)

17. Sync the changes.

    ![A screenshot of a computer Description automatically
    generated](./media/image53.png)

18. Navigate to GitHub repository and open the **octofit_tracker**
    folder inside octofit-tracker/backend folder.

    ![A screenshot of a computer Description automatically
    generated](./media/image54.png)

19. Now, all files have been pushed to GitHub.

    ![A screenshot of a computer Description automatically
    generated](./media/image55.png)

### Exercise 4: Setup Django REST Framework, start the server, and test the API

1. Navigate back to Codespace and open a new chat by clicking on **+**
    icon.

    ![A screenshot of a computer Description automatically
    generated](./media/image56.png)

2. Setup Django REST Framework and test the REST API endpoints. Paste
    the following text on the text editor:
    ```
    Let's setup codespace for the URL, start the server via VS Code launch.json, and test the API. 

    - All Django project files are in the `octofit-tracker/backend/octofit_tracker` directory. 

    - Only update urls in `settings.py` and `urls.py` 

    - REST api endpoint format https://$CODESPACE_NAME-8000.app.github.dev/api/[component]/ 

    - example full url: https://$CODESPACE_NAME-8000.app.github.dev/api/activities/ 

    - Do not hard code the `$CODESPACE_NAME` value use the variable 

    - Do not update the `views.py` 

    1. Update `urls.py` to replace the return for the REST API URL endpoints with the environment variable $CODESPACE_NAME https://$CODESPACE_NAME-8000.app.github.dev for Django and avoid certificate HTTPS issues. 

    2. Make sure the Django backend works on your codespace URL and localhost (i.e., the value of `$CODESPACE_NAME`) by updating `ALLOWED_HOSTS` in `settings.py`. 

    3. Test the API endpoints using curl command. 
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image57.png)

3. If you get the below output, click on Allow.

    ![A screenshot of a computer Description automatically
    generated](./media/image58.png)

4. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image59.png)

5. Click on **Allow**.

    ![A screenshot of a computer Description automatically
    generated](./media/image60.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image61.png)

6. To run the Django application, select the Run and Debug tab.

    ![A screenshot of a computer Description automatically
    generated](./media/image62.png)

7. Select Run **Django Server** from the dropdown and select the
    **play** button beside the dropdown.

    ![A screenshot of a computer Description automatically
    generated](./media/image63.png)

8. You’ll receive a pop-up to open the application on the browser.
    Click on **Open in Browser**.

    ![A screenshot of a computer Description automatically
    generated](./media/image64.png)

9. A new tab will open on your browser. Click on **Continue** to Allow.

    ![A screenshot of a computer Description automatically
    generated](./media/image65.png)

10. Once you click Continue it should look similar the following with
    your codespace name:

    ![A screenshot of a computer Description automatically
    generated](./media/image66.png)

11. Navigate to **Source Control** tab from the left pane. 
    ![A screenshot of a computer Description automatically
    generated](./media/image67.png)

12. Write a short **commit message** and click on **Commit** button.
    ![A screenshot of a computer Description automatically
    generated](./media/image68.png)

13. Click on **Sync Changes**.

    ![A screenshot of a computer Description automatically
    generated](./media/image69.png)

### Exercise 5: Setup the frontend React framework and start OctoFit Tracker app

1. **Open a new chat** by clicking on the **+** icon from the top of
    the chat.

    ![A screenshot of a computer Description automatically
    generated](./media/image70.png)

2. Install the octofit-tracker frontend React framework, then paste the
    following text into the editor:
    ```
    Let's setup the octofit-tracker frontend React framework and

    ensure everything is created in the \`octofit-tracker/frontend\`
    directory by using \`--prefix\`

    1. Make sure the the octofit-tracker/frontend directory exists.

    2. create the react app

    3. Install react, bootstrap, and react-router-dom

    4. Import bootstrap css in the src/index.js file.

    5. Don't change .gitignore file
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image71.png)

3. Click on **Allow** button.

    ![](./media/image72.png)

4. Click on Allow button.

    ![A screenshot of a computer Description automatically
    generated](./media/image73.png)

    ![](./media/image74.png)

5. Update the octofit-tracker frontend React components, then paste the
    following text into the editor:
    ```
    Let's update the octofit-tracker frontend React components. 

    - Update the following components to include the React framework to point to the backend REST API: 

    - src/App.js 

    - src/index.js 

    - src/components/Activities.js 

    - src/components/Leaderboard.js 

    - src/components/Teams.js 

    - src/components/Users.js 

    - src/components/Workouts.js 

    - In each component replace the fetch url with the codespace url 

    https://$REACT_APP_CODESPACE_NAME-8000.app.github.dev/api/[component]/ 

    for the Django rest framework backend. 

    make sure all components are pulling data from the REST api endpoint 

    for display in the REACT frontend 

    - Make sure to use the correct port and protocol http or https. 

    - Update src/App.js to include the main navigation for all components. 

    - Make sure react-router-dom is used for the navigation menu. 

    - The react app should show the navigation menu and the components. 

    - Update all components to log the fetched data and make them compatible with both paginated (.results) and plain array responses. 

    - Add console.log statements to each component to log the fetched data and the REST API endpoints. 
    ```
    ![A screenshot of a computer Description automatically
    generated](./media/image75.png)

6. Click on Allow.

    ![A screenshot of a computer Description automatically
    generated](./media/image76.png)

7. Click on Allow.

    ![A screenshot of a computer Description automatically
    generated](./media/image77.png)

8. Click on Yes button.

    ![A screenshot of a computer Description automatically
    generated](./media/image78.png)

9. Click on **Allow** button.

    ![A screenshot of a computer Description automatically
    generated](./media/image79.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image80.png)

10. To run the Django application, select the **Run and Debug** tab.

    ![A screenshot of a computer Description automatically
    generated](./media/image81.png)

11. From the dropdown, select **Launch React Frontend** option and click
    on the **play** button.

    ![A screenshot of a computer Description automatically
    generated](./media/image82.png)

12. Click on **Open in Browser**.

    ![A screenshot of a computer Description automatically
    generated](./media/image83.png)

13. Click on **Continue** to Allow.

    ![A screenshot of a computer Description automatically
    generated](./media/image84.png)

14. Once you click **Continue** it should look similar the following:

    ![A screenshot of a computer Description automatically
    generated](./media/image85.png)

15. Let's add some formatting, structuring, and styling to the octofit
    tracker app, then paste the following text into the editor:
    ```
    Let's update the octofit-tracker frontend React components. 

    - Update the following components to include the React framework to point to the backend REST API: 

    - src/App.js 

    - src/index.js 

    - src/components/Activities.js 

    - src/components/Leaderboard.js 

    - src/components/Teams.js 

    - src/components/Users.js 

    - src/components/Workouts.js 

    - In each component replace the fetch url with the codespace url 

    https://$REACT_APP_CODESPACE_NAME-8000.app.github.dev/api/[component]/ 

    for the Django rest framework backend. 

    make sure all components are pulling data from the REST api endpoint 

    for display in the REACT frontend 

    - Make sure to use the correct port and protocol http or https. 

    - Update src/App.js to include the main navigation for all components. 

    - Make sure react-router-dom is used for the navigation menu. 

    - The react app should show the navigation menu and the components. 

    - Update all components to log the fetched data and make them compatible with both paginated (.results) and plain array responses. 

    - Add console.log statements to each component to log the fetched data and the REST API endpoints. 
    ```

    ![A screenshot of a computer Description automatically
    generated](./media/image86.png)

16. You’ll get the below output.

    ![A screenshot of a computer Description automatically
    generated](./media/image87.png)

17. Navigate to **Source Control** tab to commit the changes. Add the
    **commit** **message** and click on **Commit** button.

    ![A screenshot of a computer Description automatically
    generated](./media/image88.png)

18. Click on **Yes**.

    ![A screenshot of a computer Description automatically
    generated](./media/image89.png)

19. Click on **Sync Changes**.

    ![A computer screen shot of a computer screen Description
    automatically generated](./media/image90.png)

20. Navigate to the repository and open **octofit-tracker** folder.

    ![A screenshot of a computer Description automatically
    generated](./media/image91.png)

21. Open the **frontend** folder.

    ![A screenshot of a computer Description automatically
    generated](./media/image92.png)

    ![A screenshot of a computer Description automatically
    generated](./media/image93.png)

22. Now, navigate to the **React application** browser tab and refresh
    the page. You’ll see the application is now updated with bootstrap.

    ![A screenshot of a computer Description automatically
    generated](./media/image94.png)

## Summery

In this lab, participants learned how to use GitHub Codespaces and
Copilot agent mode to build a complete full-stack web application.
They gained hands-on experience setting up a Django backend with
MongoDB, creating and testing REST APIs, and developing a responsive
React frontend. By the end, participants successfully integrated all
components into a functional and styled OctoFit Tracker app.
