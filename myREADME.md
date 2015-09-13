##Prestashop webapp for educational purposes.  
* Create a new PHP+MySQL workspace from c9 
* Install and start MySQL ```mysql-ctl start```. You'll give a root user, (c9's username, ```algurpe``` in this case) and a DataBase name, (c9)  
* Upload prestashop unzipped files to c9  and unzip it.
* Click on ```Run``` c9IDE's button to open ```index.php``` file and start the installation process and follow the instructions:  
   1. Select your language --> ```Next```  
   2. Accept the License: check and ```Next```  
   3. Fill the store info: name, activity, etc..., Set up Account: name, email, password,... -> ```Next```  
   4. Set up database: server path (localhost), db name (c9), dbuser (root), passwd (empty), tableprefix (ps_). Try the connection and ```Next```  
   5. The connection wizard installs the app and remembers you the email and passwd you've choosen and that you must remove the ```install``` folder.   
   6. In order to secure your app, besides removing that ```install``` folder, you might rename the ```admin``` folder to ```admin8000``` in this case, and set your policy to permmission level of files and folders (for another moment)  
   7. You're done. Now go to the URL domain.com/admin8000  to log in Prestashop's back-office  
* Open the link in the output panel to install and access your app  
