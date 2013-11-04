How to add multiple user's from a file in ubuntu 

This will Automatically Create the Username, Password , Gecos Filed, Home Directory, And Shell Which need to use
And this Option Won't add the user's to Sudoer Group too , We need to do it manually 


Step 1

Create a file names users
And Append the following users

```
user1:admin123$:::User1 Is Here:/home/user1:/bin/bash
user2:admin123$:::User2 Is Here:/home/user2:/bin/bash
user3:admin123$:::User3 Is Here:/home/user3:/bin/bash
user4:admin123$:::User4 Is Here:/home/user4:/bin/bash
user5:admin123$:::User5 Is Here:/home/user5:/bin/bash
user6:admin123$:::User6 Is Here:/home/user6:/bin/bash
user7:admin123$:::User7 Is Here:/home/user7:/bin/bash
user8:admin123$:::User8 Is Here:/home/user8:/bin/bash
user9:admin123$:::User9 Is Here:/home/user9:/bin/bash
user10:admin123$:::User10 Is Here:/home/user10:/bin/bash
user11:admin123$:::User11 Is Here:/home/user11:/bin/bash
user12:admin123$:::User12 Is Here:/home/user12:/bin/bash
user13:admin123$:::User13 Is Here:/home/user13:/bin/bash
user14:admin123$:::User14 Is Here:/home/user14:/bin/bash
user15:admin123$:::User15 Is Here:/home/user15:/bin/bash
user16:admin123$:::User16 Is Here:/home/user16:/bin/bash
user17:admin123$:::User17 Is Here:/home/user17:/bin/bash
user18:admin123$:::User18 Is Here:/home/user18:/bin/bash
user19:admin123$:::User19 Is Here:/home/user19:/bin/bash
user20:admin123$:::User20 Is Here:/home/user20:/bin/bash
```

Save and Close Using wq!


Step 2

Use command 

```
#newusers < users

```

This will Create the user's With provided Password from the file

Note : This will not copy the skel files from /etc/skel/bashprofiles files


Step 3 


Copy the Skel file's Manulally Using 

```
# cp -R /etc/skel/ ~/
```

Step 4 

Man Page

 
```
# man newusers
```

Thats it ...

