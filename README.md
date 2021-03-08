# install aws-amplify
npm i aws-amplify

# SETUP following properties in your config file  ( check config.json for reference )

    "cognito": {
        "REGION":"us-east-1",
        "USER_POOL_ID":"us-east-1_NUWemQsIS",
        "APP_CLIENT_ID":"6s8t7kp8qqvuu4ht02puli6ui1"
  }

#  Configure aws amplify auth to your react app ( check index.js for reference)

import Amplify from 'aws-amplify'
import config from './config';

Amplify.configure({
    Auth:{
        mandatorySignId: true,
        region: config.cognito.REGION,
        userPoolId: config.cognito.USER_POOL_ID,
        userPoolWebClientId: config.cognito.APP_CLIENT_ID
    }
});

# ######################################################################
# Signup:  Add the following code to your signup form submit handler ( check auth/Register.js for reference)


import { Auth } from 'aws-amplify';

    const { email, password } = this.state;
    try{
      const signUpResponse = await Auth.signUp({
        username:email,
        password
      });

      console.log(signUpResponse);
      this.props.history.push("/welcome");  
      
    } catch(error) {
      // Handle error
    }

# Please show pws requirments minimum length 8. and require number, special, upper and lowercase characters.
# Please redirect to the page where it says that: A verification link has been sent to your email. Verify the link to complete signup

# ############################################################
# Login: Add the following code to your login form submit handler ( check auth/LogIn.js for reference)

import {Auth} from "aws-amplify";

    try{
      const user = await Auth.signIn(this.state.email, this.state.password);

      console.log(user);
      this.props.history.push("/");
      
    } catch(error) {
      // handle errro
    }


# ################################################################
# Logout: add the following code to logout onclick event handler (check auth/Navbar.js for reference)

import {Auth} from "aws-amplify";

    try {
      Auth.signOut();
     // update the authenticated status and set user to null
    } catch(error){
      // handle error
    }

# #####################################################
# Session Persistence: To save authentication info in the local storage add the following code (check App.js for reference). AWS cognito automatically handle authenticated session

import { Auth } from 'aws-amplify';

    try {
      const session = await Auth.currentSession();
      this.setAuthStatus(true);
      console.log(session);
      const user = await Auth.currentAuthenticatedUser();
      this.setUser(user);
    } catch(error){
      console.log(error);
    }

# #########################################################
# Forgot password: Add the following code to the form handler  (check Auth/ForgetPassword.js for reference)

import { Auth } from 'aws-amplify';

  try {
      await Auth.forgotPassword(this.state.email);
      this.props.history.push('/forgotpasswordverification');
    } catch(error){
      console.log(error);
    }

# ###########################################################
# Set new password: Add the following code to setup new password (check Auth/ForgotPasswordVerification.js for reference)

import { Auth } from 'aws-amplify';

try{
      await Auth.forgotPasswordSubmit(
        this.state.email,
        this.state.verificationcode,
        this.state.newpassword
      )
      this.props.history.push("/changepasswordconfirmation");

    }catch(error){
        console.log(error);
    }

# ##############################################################
# Change password: Add the following code if user wants to chane password. Check Auth/ChangePassword.js for reference)


import { Auth } from 'aws-amplify';

try {
      const user = await Auth.currentAuthenticatedUser();
      console.log(user);
      await Auth.changePassword(
        user,
        this.state.oldpassword,
        this.state.newpassword
      );
      this.props.history.push("/changepasswordconfirmation");
    }catch(error){
      console.log(error);
    }