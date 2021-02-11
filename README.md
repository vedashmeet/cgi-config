## Common Gateway Interface Configuration on Linux Ubuntu

##### For Github Page ([https://vedashmeet.github.io/cgi-config/](https://vedashmeet.github.io/cgi-config/))
##### For Github Repository ([https://github.com/vedashmeet/cgi-config/](https://github.com/vedashmeet/cgi-config/))

Common Gateway Interface provides the middleware between WWW servers and external databases and information source. The Web server typically passes the form information to a small application program that processes the data and may send back a confirmation message. This process or convention for passing data back and forth between the server and the application is called the common gateway interface (CGI).
    For this, I have develop a simple C program for printing a multiplication table that run on a web server and display output on any browser.

### Steps for Configuration of CGI:

- **Update and Upgrade** your apt package with the help of following command, then press enter.

```markdown
$ sudo apt update && upgrade
```

- Install **APACHE** web server on your Linux environment with the help of following command.

```markdown
$ sudo apt install apache2
```

- Start **APACHE** services.

```markdown
$ sudo systemctl start apache2.service
```

- Install GCC compiler for compiliing C program using following command.

```markdownn
$ sudo apt install gcc
```

  If you want to use any other language instead of C, then you have to install compiler for that language. For getting the command to install the compiler of any other language, you can simply google it.

- Create folder/directory where you placed your programs/projects, that is accessible by web server.

```markdown
$ sudo mkdir /home/pf/cProgram/
```

  Here, **/home/pf/cProgram/** is the directory where you can store your project/programs. You can label your directory with any name. To check your directory path, simply enter into your directory wherever it is placed using **$ cd directory-name**, then enter following command, you will get your current working directory.
 
```markdown
$ pwd
```

- Create your C program file in any editor such as **nano** or **vim**, etc.

```markdown
$ nano /home/pf/cProgram/printTable.c
```

or
```markdown
$ vim /home/pf/cProgram/printTable.c
```

  Here **/home/pf/cProgram/** is the directory path and **printTable.c** is the name of C program file, ending with C extension. You can give any name to file. The extension relates to which language you used for developing the programs. The program for printing multiplication table is as like,

```markdown

#include<stdio.h>
void main() {
  int tableOf = 7;
  printf("Content-type: text/html\n\n");
  printf("<html>\n");
  printf("<head>\n");
  printf("<title>Table of %d</title>\n", tableOf);
  printf("</head>\n");
  printf("<body style = margin:15px>\n");
  printf("<h2>Table of %d:\n</h2>", tableOf);
  for(int count = 1; count <= 10; count++) {
    printf("<h1>%d X %d = %d\n</h1>", tableOf, count, tableOf * count);
  }
  printf("</body>\n");
  printf("</html>\n");
}
                                
```


- Give executive permission to working directory, so programs inside it can be execute.

```markdown
$ sudo chmod +x /home/pf/cProgram/*
```

You can replace the directory path with your own.

- Compile the program file.

```markdown
$ sudo g++ /home/pf/cProgram/printTable.c -o /home/pf/cProgram/printTable
```

  Here, **/home/pf/cProgram/printTable.c** is compiled with **g++** compiler and output of compiling stored in **/home/pf/cProgram/printTable**. If you not used **-o** for storing output of compilation in another file, then by default it stored in file named as **a.out**.

- Run the program.

```markdown
$ /home/pf/cProgram/./printTable
```

or

```markdown
$ /home/pf/cProgram/./a.out
```

Here, **printTable** and **a.out** is the file name that stored the compilation result of program.

- Edit **APACHE** configuration file.

```markdown
$ sudo vim /etc/apache2/apache2.conf
```

Add **Directory** directie for **/home/pf/cProgram/** into file named as **apache2.conf**.

```markdown
<Directory /home/pf/cProgram/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

You can change /home/pf/cProgram/ to your's directory path.

- Check available CGI file in your system. Default CGI file is placed in the following diretory.

```markdown
$ sudo vim /etc/apache2/conf-available/serve-cgi-bin.conf
```

You can get your system CGI file path from here, that might be **/usr/lib/cgi-bin/**. This sure that your system support the CGI.

- You can create your own CGI configuration file, then enable it.

```markdown
$ sudo vim /etc/apache2/conf-available/mine-cgi-bin.conf
```

Here, **/etc/apache2/conf-available/** is CGI configuration file's directory and **mine-cgi-bin.conf** is my own CGI configuration file ending with extension **.conf** for configuration. You can create your owns. The CGI script in file is as,

```markdown
# ScriptAlias tells Apache that a /home/pf/cProgram directory is set aside for CGI programs
ScriptAlias / /home/pf/cProgram/

# To apply a group of directives to my directory "/home/pf/cProgram"
<Directory "/home/pf/cProgram">
        Options +ExecCGI
        AddHandler cgi-script .cgi .c
</Directory>
```

You can replace **/home/pf/cProgram/** with you directory path. Add extension of languages after **.cgi .c ...** whose code you want to execute through CGI.

- Enable CGI file that you have created.

```markdown
$ sudo a2enconf mine-cgi-bin
```

Here, **mine-cgi-bin** is the name of CGI configuration file. You can replace it with your own.

- Enable CGI module.

```markdown
$ sudo a2enmod cgi
```

- Restart **APACHE** services.

```markdown
$ sudo systemctl restart apache2.service
```

- Open browser and search URL **localhost/printTable**. Here, **prntTable** is the name of the executable file, you can replace it with your own. And if you want to run this program on any other client system, then replace **localhost** with your system **IP Address**. You can simply check your IP address on terminal with command **$ ifconfig**.

- If you want to stop **APACHE** services, then run following command.

```markdown
$ sudo systemctl stop apache2.service
```
