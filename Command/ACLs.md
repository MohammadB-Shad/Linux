# ACLs
This type of situation is what Linux Access Control Lists (ACLs) were intended to resolve. ACLs allow us to apply a more specific set of permissions to a file or directory without (necessarily) changing the base ownership and permissions. They let us "tack on" access for other users or groups.

# Basic Syntax
The `setfacl` command in Linux is used to ACLs on files and directories.

```
setfacl [option] file_or_directory
```

# Command Options
  - '-m' Modify the ACL.This is often used to add or modify entries in the ACL.
  - '-x' Remove the specified ACL entry.
  - '-b' Remove all ACL entries.

# Command Access
  - 'r' This grants Read. 
  - 'w' This grants Write
  - 'X' This grants Excute

# Command Level
  - 'u' This For Users Level.
  - 'g' This For Groups Level.

# For Examples
  # Add Access
  - Add Full (Read,Write,Excute) Access for a user:
  ```
  setfacl -m u:temp_user:rwX file_or_directory
  ```
  - Add Full (Read,Write,Excute) Access for a group:
  ```
  setfacl -m g:temp_group:rwX file_or_directory
  ```
  # Remove Access
  - Remove ACL Entry 
  ```
  setfacl -x u:temp_user file_or_directory
  ```

  - Remove All ACLs
  ```
  setfacl -b file_or_directory
  ```

# Additional Notes
- Default ACLs:
  - You can also set default ACLs that will be inherited by new files and subdirectories created within a directory.
 
- Viewing ACLs:
  - You can view ACLs using the 'getfacl' command.
  ```
  getfacl file_or_directory
  ```

For More Detail using the 'man setfacl' command.
```
man setfacl
```

GeekShad - GoodLuck!
    
