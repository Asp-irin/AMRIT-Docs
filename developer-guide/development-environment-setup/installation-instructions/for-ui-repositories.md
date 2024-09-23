# For UI repositories

**Steps to Clone and Set Up the UI Repository**

1. **Clone the Repository:**
   *   Clone the UI repo from the GitHub fork branch to your local system using the command:

       ```bash
       git clone <repository-url>
       ```
2. **Open the Project:**
   * Open the project in Visual Studio Code/your preferred IDE.
3. **Install Node Modules:**
   *   Navigate to your project folder and install the necessary Node.js modules using:

       ```bash
       npm install
       ```
4. **Copy Environment Configuration:**
   *   Copy the environment configuration file:

       ```bash
       cp src/environments/environment.ts src/environments/environment.local.ts
       ```
   * Edit the endpoints, ports, and IPs in `environment.local.ts` as per your local running services.
5. **Run the Project:**
   *   Once the Node modules are installed successfully, run the project using:

       ```bash
       ng serve
       ```

By default, your application will be available at `http://localhost:4200/`. You can access it in your browser.
