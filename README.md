# My Portfolio using the Launch Darkly SDK to enable a Feature Flag
This app is a React App that I wrote for my Portfolio. The app has been updated to import the LaunchDarkly SDK to enable a Feature Flag

## Steps to install and run the app
## Using the create-react-app command
The 'create-react-app' is a tool maintained by Facebook itself and can be used to install react. This is suitable for beginners without manually having to deal with transpiling tools like webpack and babel.

### 1. Install Node JS and npm
* Checking your version of npm and Node.js
To see if Node.js and npm have already been installed and check the installed version, run the following commands:

`node -v`

`npm -v`

### Using a Node version manager to install Node.js and npm
Node version managers allow installation and switching between multiple versions of Node.js and npm on the system so applications can be tested on multiple versions of npm to ensure they work for users on different versions.

#### OSX or Linux Node version managers
* [nvm](https://github.com/creationix/nvm)
* [n](https://github.com/tj/n)

#### Windows Node version managers
* [nodist](https://github.com/marcelklehr/nodist)
* [nvm-windows](https://github.com/coreybutler/nvm-windows)

### Using a Node installer to install Node.js and npm
Node installer can be used to install both Node.js and npm locally if Node version manager seems difficult to use.

* [Node.js installer](https://nodejs.org/en/download/)
* [NodeSource installer](https://github.com/nodesource/distributions)

#### OS X or Windows Node installers
For OS X or Windows, use one of the installers from the [Node.js download page](https://nodejs.org/en/download/). Be sure to install the version labeled LTS. Other versions have not yet been tested with npm.

#### Linux or other operating systems Node installers
For Linux or another operating system, use one of the following installers:

* [NodeSource installer](https://github.com/nodesource/distributions) (recommended)
* One of the installers on the [Node.js download page](https://nodejs.org/en/download/)

### 2. Install React
React can be installed using npm package manager by using the below command.

`npm install -g create-react-app  `

### 3. Run the App
After completing steps 1 and 2, start the server by running the following command.

Navigate to the directory that has the react app.
Run the following command

`npm start`

The app will look like this 
![Screen Shot 2021-08-06 at 1 18 30 AM](https://user-images.githubusercontent.com/10985717/128480004-a5e458df-12d1-4b39-9358-b742b0c5aa64.png)

## Add the Feature Flag using Launch Darkly SDK
### 1. Sign up for an account with Launch Darkly
Goto [Sign up page](https://app.launchdarkly.com/signup) and sign up to create a trial account with Launch Darkly.

### 2. Create a feature flag
* Goto <https://app.launchdarkly.com/default/test/quickstart> and create a feature flag with the name `text-search`.
* In the section _Set up the applicaation_ select React from the dropdown _Select your language_ and Test from the dropdown _Select your environment_
* Step a is to create a react app. Since this has already been completed in the previous steps, it is not required to create a react app again.
* Install the LaunchDarkly SDK
  
  `npm install --save launchdarkly-react-client-sdk@2.23.0`
* In App.js, import withLDProvider:
 ```javascript 
 import { withLDProvider } from 'launchdarkly-react-client-sdk';
 ```
* Then, change the export default to use your environment-specific client ID and a sample user:
```javascript 
export default withLDProvider({
  clientSideID: 'CLIENT ID HERE',
  user: {
      "key": "user_key",
      "name": "User Name",
      "email": "User@email.com"
  }
})(App);
```
* Add a new React component [FeatureFlag.js](https://github.com/Siddhartha1193/portfolioFeatureFlag/blob/main/src/components/FeatureFlag.js) under the [component directory](https://github.com/Siddhartha1193/portfolioFeatureFlag/tree/main/src/components)
    * Import React and withLDConsumer
    ```javascript
    import { withLDConsumer } from 'launchdarkly-react-client-sdk';
    ```
    * Create the FeatureFlag component that gets passed the `flags` prop and returns an element based on the state of `text-search`
    ```javascript
    const FeatureFlag = ({ flags }) => {
      return flags.textSearch ? 
      <div className="section section-about section-first row">
        <h3  className="css3-gradient-2">LaunchDarkly Flag Status</h3>
          <ul className="about-list fa-ul">
            <li><span className="fa-li"><i className="fas fa-certificate "></i></span><b>Text Search Flag is on</b></li></ul></div> : 
      <div className="section section-about section-first row">
        <h3  className="css3-gradient-2">LaunchDarkly Flag Status</h3>
          <ul className="about-list fa-ul">
            <li><span className="fa-li"><i className="fas fa-certificate "></i></span><b>Text Search Flag is off</b></li></ul></div>;
    };
    
    export default withLDConsumer()(FeatureFlag);
    ```
    * Go back to [App.js](https://github.com/Siddhartha1193/portfolioFeatureFlag/blob/main/src/App.js) and import the FeatureFlag component
    ```javascript
    import FeatureFlag from './components/FeatureFlag';
    
    class App extends Component {
      render() {
       return (
         <div className="App">

          <div className="container">
            <div className="row page-wrap">
              <Header/>
              <Sidebar/>
              <div className="span8 main-wrap">
                <About/>
                <Experience/>
                <Education/>
                <Skills/>
                <Projects/>
                <FeatureFlag/>

              </div>

            </div>
          </div>
        </div>
      );
    }
  }
  ```
 ### 3. Run the app to test the feature flag
Run the following command to run the app

`npm start`

The feature flag will get added as another component to the app at the bottom of the webpage 

* **When the feature flag `text-search` is turned on**
<img width="1168" alt="Screen Shot 2021-08-06 at 11 25 00 AM" src="https://user-images.githubusercontent.com/10985717/128573186-5f687f6d-6274-4150-892b-a98946f2362a.png">

**React app-**

![Screen Shot 2021-08-06 at 11 25 10 AM](https://user-images.githubusercontent.com/10985717/128573252-92d9c7a2-1c03-4ec7-8148-0b0707f4d639.png)

* **When the feature flag `text-search` is turned off**
<img width="1159" alt="Screen Shot 2021-08-06 at 11 25 41 AM" src="https://user-images.githubusercontent.com/10985717/128573286-5202bfc9-51ab-4775-9706-a2ba9ac8094b.png">

**React app-**

![Screen Shot 2021-08-06 at 11 23 35 AM](https://user-images.githubusercontent.com/10985717/128573157-0cf14817-59a5-40ec-be99-2adc6e77d1ba.png)


