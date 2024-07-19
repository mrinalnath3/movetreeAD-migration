# MovetreeAD-migration command how to make to work on windows server 2019 
AD user account migration within the forest using native tools movetree.exe command. Example child domain to child or child to root domain.
This command-line tool allows administrators to move Active Directory objects such as organizational units and users between the domains within a single forest. These operations are performed to support domain consolidation or organizational restructuring operations.
Movetree.exe ships with Windows 2000 Support Tools
Files required:
Movetree.dll
Movetree.exe
Download the below files from repository 
WindowsServer2003-KB340178-SP2-x86-ENU
Copy Movetree.dll to C:\Windows\SysWOW64
Copy Movetree.exe to C:\Windows\System32
install Adminpack WindowsServer2003-KB340178-SP2-x86-ENU

MoveTree returns ErrorLevel 0 for success and ErrorLevels 1 to 5 for different kinds of failures.

MoveTree [/start | /continue | /check] [/s SrcDSA] [/d DstDSA]
         [/sdn SrcDN] [/ddn DstDN] [/u Domain\Username] [/p Password] [/verbose]

  /start        : Start a move tree operation with /check option by default.
                : Instead, you could be able to use /startnocheck to start a move
                : tree operation without any check.
  /continue     : Continue a failed move tree operation.
  /check        : Check the whole tree before actually move any object.
  /s <SrcDSA>   : Source server's fully qualified primary DNS name. Required
  /d <DstDSA>   : Destination server's fully qualified primary DNS name. Required
  /sdn <SrcDN>  : Source sub-tree's root DN.
                : Required in Start and Check case. Optional in Continue case
  /ddn <DstDN>  : Destination sub-tree's root DN. RDN plus Destinaton Parent DN. Required
  /u <Domain\UserName>  : Domain Name and User Account Name. Optional
  /p <Password> : Password. Optional
  /verbose      : Verbose Mode. Pipe anything onto screen. Optional

EXAMPLES:

  movetree /check /s Server1.Dom1.Com /d Server2.Dom2.Com /sdn OU=foo,DC=Dom1,DC=Com
           /ddn OU=foo,DC=Dom2,DC=Com /u Dom1\administrator /p *

  movetree /start /s Server1.Dom1.Com /d Server2.Dom2.Com /sdn OU=foo,DC=Dom1,DC=Com
  /ddn OU=foo,DC=Dom2,DC=Com /u Dom1\administrator /p MySecretPwd

  movetree /startnocheck /s Server1.Dom1.Com /d Server2.Dom2.Com /sdn OU=foo,DC=Dom1,DC=Com
           /ddn OU=foo,DC=Dom2,DC=Com /u Dom1\administrator /p MySecretPwd

   User’s password must be of the length required by the destination domain. For example, if the user has a 12-characters password as required by the source domain destination domain requires a 12-digit password
   Verify the user’s group membership before moving a user account. Global groups are limited to having members from the domain the group exists in.

   There is one exception to this rule: If a user’s primary group is the Domain Users global group, and the user isn’t a member of any other global groups, the move will be successful. 
   The best practice to export the user's group membership and remove the group membership before executing the move tree command.

  movetree /continue /s Server1.Dom1.Com /d Server2.Dom2.Com /ddn OU=foo,DC=Dom1,DC=Com
           /u Dom1\administrator /p * /verbose

movetree /start /s NA-BECOMADDS03.na.becomnet.com /d BLR-BECOMADDS01.becomnet.com /sdn "CN=test\, move,ou=Becomusersou,DC=na,DC=becomnet,DC=com" /ddn "CN=test\, move,OU=Becomnetusers,DC=becomnet,DC=com" /verbose
