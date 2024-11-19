The frontend deployment of The Oruggin Trail was done via Vercel

### Deployment Process
1. Create an account on Vercel at https://vercel.com
2. On the right side there is a buttom called "Add New". Click it and select the option 'Project'
    2.1 This will allow to import a Git Repository as well from a template. Select Github and connect to you account or organization. 
    2.2 If you are connecting to an organization, you will be allowed to connect to all or a specific repository. In our case it was selected a specific repository (TheOrugginTrail_FE_Svelte).
3. Once Vercel is given authorisation, you will be able to configure the project such as:
    3.1 specifying the name and the framwork,
    3.2 the build and output settings: here you specify the build command, the output directory and the install command. 
        3.2.1 In our project, the build command was 'npm rund build' and the install command was 'npm install'
    3.3 and environment variables.
4. Once you have configured the project, click the 'Deploy' buttom. The deployment will take some time.

### Other configurations
Once the deployment is done some further adjustments were made on the project settings

## Domains
Here was specified to which domain the project was linked to. In our case it is https://itrainspiders.co.uk

## Environment Variables
Here was added an enviroment variable for the backend server. Give it a key name and the value. In our case the key name was "Backend_URL" and the value was set to be the IP from the EC2 instance.

In case the IP from the EC2 instance changes, it is necessary to update the value given before to the new one.

## Deployment Protections
Here on the OPTIONS Allowlist, it was enabled and added two paths
1. The backend address with the port for the connection to the game. In our case it was '/meatpuppet.itrainspiders.co.uk:7348'
2. The backend address. In our case it was '/meatpuppet.itrainspiders.co.uk'

