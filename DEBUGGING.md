Troubleshooting
===============

* Always start your network fresh. Use the script `deleteNetwork.sh` to delete any pre-existing jobs/pods etc.

* If you see below error, then environment is not set properly and therefore kubectl commands will not work.
  ```
  $ kubectl get pods
  The connection to the server localhost:8080 was refused - did you specify the right host or port?
  ```
  
* If you see below error, it means this peer has not joined the channel and so your query will not work.
  ![](images/error1.png)
  
* If you see something similar to the following:
  ![](images/error2.png)
  
  It shows there is some error in command. For example, in this snapshot `-c` is missing before passing arguments.
  
* If you see the below error,
  ![](images/error3.png)
  
  There is something wrong with the setup. You would like to do setup from a fresh.
  
