# Public private key
Needed when connecting VSCode directly to remote server which generally needs login thorough a proxy portal.
Steps:

1. Open up PowerShell on your local computer and run `ssh-keygen`. The default path for your keys is `C:\users\<user>\.ssh`.
2. Provide the folder path to save the private and public key. The default is `C:\Users\<user>\.ssh\id_rsa`.
3. Provide an optional passphrase. If you provide a passphrase, this passphrase will be used to encrypt the private key.
4. When complete, you’ll now have two files (keys) in the folder you saved the keys into called id_rsa.pub (public key) and id_rsa (private key). By default, these keys will be in the C:\Users\<user>\.ssh folder.
5. Copy the key in id_rsa.pub file.
6. In your remote server, go to your home folder and save the key in .ssh/authorized_keys file using `echo key > .ssh/authorized_keys`.
7. Now press F1 in vscode, connect to remote host, type the host link and vscode will as for the passphrase if you have provided any.