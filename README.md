# githubdeploy
This repository is demonstrating the steps to create the build and deploy workflow for DotNet core projects to Azure.
## step 1
Creating web site in Azure.
![creating web site in Azure](https://github.com/mkokabi/githubdeploy/blob/master/images/image1.png)

## step 2
Creating a dotnet core web application
![Creating a dotnet core web application](https://github.com/mkokabi/githubdeploy/blob/master/images/image2.png)
For this sample I named the web application *github*

## step 3
Pushing the web application to github.
![Getting the remote URL](https://github.com/mkokabi/githubdeploy/blob/master/images/image3.png)

```shell
cd github
git init
git remote add origin https://github.com/mkokabi/githubdeploy.git
git pull origin master
git add .
git commit -m "First commit"
git push --set-upstream origin master
```
### step 4
Get the publish profile of your Azure web app.
![Get the publish profile](https://github.com/mkokabi/githubdeploy/blob/master/images/image6.png)

### step 5
Store the contents of the publish profile in the repository secret with a name such as azureWebAppPublishProfile.
![Creating repository secret](https://github.com/mkokabi/githubdeploy/blob/master/images/image7.png)


### step 6
Creating workflow
![Switching to actions tab](https://github.com/mkokabi/githubdeploy/blob/master/images/image4.png)

### step 7
Select start a workflow yourself
![Start a new workflow](https://github.com/mkokabi/githubdeploy/blob/master/images/image5.png)

### step 8
Replace the steps with
```yaml
      - uses: actions/checkout@v2
        name: checkout
      - uses: actions/setup-dotnet@v1
        with:
          dotnet-version: '3.1.100' # SDK Version to use.
      - run: dotnet build --configuration Release

      - name: dotnet publish
        run: |
          dotnet publish -c Release -o ${{env.DOTNET_ROOT}}/github 
      - name: 'Run Azure webapp deploy action using publish profile credentials'
        uses: azure/webapps-deploy@v1
        with: 
          app-name: GithubDotNetCore  # Replace with your app name
          publish-profile: ${{ secrets.azureWebAppPublishProfile }} 
          package: ${{env.DOTNET_ROOT}}/github 
```
- The *github* in dotnet publish command is the name of my web application. You can replace it with your project name. 
- The *GithubDotNetCore* in the app-name parameter is the name of my Azure Web App. 


