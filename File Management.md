## Questions and Answers
1.program to create a new text file and write "Hello,world!" to it.
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void)
{
  int fd,ref;
  char buf[11]="hello world";
  fd=open("vishnu.txt",O_CREAT|O_RDWR,0666);
  printf("%d\n",fd);
  if(fd<0)
  {
    printf("open system call failed\n");
    return 0;
  }
  ref=write(fd,buf,strlen(buf));
  if(ref<0)
  {
    printf("Write system call is failed\n");
    return 0;
  }
  close(fd);
  return 0;
}
```
2.program to open an existing text file and display its contents.
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void)
{
	int fd,ref;
	char buf[1024];
	fd=open("vishnu.txt",O_RDONLY);
	//printf("%ld\n",sizeof(buf));
//	printf("%d\n",fd);
	if(fd<0)
	{
		printf("Open system call failed\n");
		return 0;
	}
	while(ref=read(fd,buf,sizeof(buf)-1))
	{
//	if(ref<0)
//	{
//		printf("Read system call failed\n");
//		close(fd);
//		return 0;
//	}
	buf[ref]='\0';
	printf("%s",buf);
	}
	close (fd);
	return 0;
}
```
3.Program to create a directory in current directory.
```c
#include<stdio.h>
#include<sys/stat.h>
#include<sys/types.h>
int main()
{
	int status=mkdir("test",0777);
	if(status==-1)
	{
		printf("Error creating directory");
		return 1;
	}
	printf("Directory 'Test' created sucessfully!\n");
	return 0;
}
```
4. Program to check whether txt file is present or not.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
	int fd=open("file.txt",O_RDONLY);
	if(fd==-1)
	{
		printf("file does not exist.\n");
	}
	else
	{
		printf("file exists");
		close(fd);
	}
	return 0;
}
```
5.Program to remove the empty directory.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	int status=rmdir("test");
	if(status==-1)
	{
		printf("Error:removing directory.\n");
	}
	else{
	printf("Directory removed sucessfully");
	}
	return 0;
}
```
6.Program to rename the old text to an new text.
```c
#include<stdio.h>
int main()
{
	int status=rename("reddy.txt","vishnu.txt");
	if(status==0)
	{
		printf("File renamed sucessfully\n");
	}
	else
	{
		printf("error ranaming file");
		return 1;
	}
	return 0;
}
```
7. Program to delete a .txt file.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main()
{
	const char *filename="file.txt";
	int ref;
	ref=unlink(filename);
	if(ref<0)
	{
		printf("error: could not delete \%s\".File may not exist or lacks permission.\n", filename);
		return 1;
	}
	printf("File \"%s\" deleted successfully.\n", filename);
	return 0;
}
```
8.program to copy the contents of one file to another.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main()
{
	int source_fd,destination_fd,bytes;
	char buffer[100];
	source_fd=open("vishnu.txt",O_RDONLY);
	if(source_fd<0)
	{
		printf("error opening source file");
		return 1;
	}
	destination_fd=open("destination.txt",O_CREAT|O_WRONLY,0666);
	if(destination_fd<0)
	{
		printf("error creating destination file");
		close(source_fd);
		return 1;
	}
	while((bytes=read(source_fd,buffer,sizeof(buffer)))>0)
	{
		write(destination_fd,buffer,bytes);
	}
	close(source_fd);
	close(destination_fd);
	printf("File Copied sucessfully\n");
	return 0;
}
```
9.Program to move a file from one directory to another directory.
```c
#include <stdio.h>
int main()
{
    const char *source = "vishnu.txt";  // Change to actual source file path
    const char *destination = "linux1/vvishnu.txt";  // Change to target location
    // Attempt to move the file
    if (rename(source, destination) == 0) {
        printf("File moved successfully from \"%s\" to \"%s\".\n", source, destination);
    } else {
        perror("Error moving file");
    }

    return 0;
}
```
10.program to print the files in an current directory.
```c
#include <stdio.h>
#include <dirent.h>
int main() {
    struct dirent *entry;
    DIR *dir = opendir(".");
    if (dir == NULL) {
        perror("Error opening directory");
        return 1;
    }
    printf("Files in the current directory:\n");
    while ((entry = readdir(dir)) != NULL) {
        printf("%s\n", entry->d_name);
    }
    closedir(dir);
    return 0;
}
```
11.Program to get the size of an file.
```c
#include <stdio.h>
#include <sys/stat.h>
int main() {
    const char *filename = "destination.txt";
    struct stat file_stat;
    // Get file information
    if (stat(filename, &file_stat) == 0) {
        printf("Size of \"%s\": %ld bytes\n", filename, file_stat.st_size);
    } else {
        perror("Error retrieving file size");
    }
    return 0;
}
```
12.Program to create a new directory in the parent directory.
```c
#include<stdio.h>
#include<sys/stat.h>
#include<errno.h>
int main()
{
	const char *dirname="../backup";
	if(mkdir(dirname,0755)==0)
	{
		printf("Directory \"%s\" created sucessfully.\n",dirname);
	}
	else
	{
		perror("Error creating directory");
	}
	return 0;
}
```
13.Program to recursivley list all files and directories in a given directory.
```c
#include<stdio.h>
#include<stdlib.h>
#include<dirent.h>
#include<sys/stat.h>
#include<string.h>
void list_directory(const char *path)
{
	struct dirent *entry;
	struct stat file_stat;
	DIR *dir=opendir(path);
	if(dir==NULL)
	{
		perror("Error opening directory");
		return;
	}
	while((entry=readdir(dir))!=NULL)
	{
		char full_path[1024];
		if(strcmp(entry->d_name,".")==0||strcmp(entry->d_name,"..")==0)
			continue;
		snprintf(full_path,sizeof(full_path),"%s/%s",path,entry->d_name);
		if(stat(full_path,&file_stat)==0)
		{
			printf("%s\n",full_path);
			if(S_ISDIR(file_stat.st_mode))
			{
				list_directory(full_path);
			}
		}
	}
	closedir(dir);
}
int main(int argc,char *argv[])
{
	const char *dir_path=(argc>1)?argv[1]:".";
	printf("Listing files and directories in: %s\n",dir_path);
	list_directory(dir_path);
	return 0;
}
```
14.Program for creating myfifpo
```
#include<stdio.h>
#include<sys/ipc.h>
int main()
{
        int ret=mkfifo("myfifpo",0666);
        if(ret<0)
        {
                printf("error in creating the fifo\n");
                return -1;
        }
        printf("fifo is created\n");
}
```
15. Program to Delete all files in an "test" directory.
```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <dirent.h>
#include <string.h>
void delete_files(const char *dir_path)
{
	struct dirent *entry;
	DIR *dir=opendir(dir_path);
	if(dir==NULL)
	{
		perror("Error opening directory");
		return ;
	}
	while((entry =readdir(dir))!=NULL)
	{
		char file_path[0124];
		if(strcmp(entry->d_name,".")==0||strcmp(entry->d_name,"..")==0)
		{
			continue;
		}
		snprintf(file_path,sizeof(file_path),"%s/%s",dir_path,entry->d_name);
		int fd=open(file_path,O_RDONLY);
		if(fd<0)
		{
			perror("Error opening file");
			continue;
		}
		close(fd);
		if(unlink(file_path)==0)
		{
			printf("Deleted: %s\n",file_path);
		}
		else{
			perror("Error deleting file");
		}
	}
	closedir(dir);
}
int main()
{
	const char *dir_path="test";
	printf("deleting all files in: %s\n",dir_path);
	delete_files(dir_path);
	return 0;
}
```
16.Program to count the number of lines in an text file.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int count_lines(const char *filename)
{
	int fd,count=0;
	char buffer;
	fd=open(filename,O_RDONLY);
	if(fd<0)
	{
		perror("Error opening file");
		return -1;
	}
	while(read(fd,&buffer,1)>0)
	{
		if(buffer=='\n')
		{
			count++;
		}
	}
	close(fd);
	return count;
}
int main()
{
	const char *filename="vishnu.txt";
	int lines=count_lines(filename);
	if(lines>=0)
	{
		printf("Number of lines in \"%s\": %d\n",filename,lines);
	}
	return 0;
}
```
17.Program to append the given text in named.txt file.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
	const char *filename= "vishnu.txt";
	const char *text="Good bye1\n";
	int fd;
	fd=open(filename,O_WRONLY|O_APPEND);
	if(fd<0)
	{
		perror("Error opening file");
		return 1;
	}
	int ref=write(fd,text,sizeof("Goodbye!\n")-1);
	if(ref<0)
	{
		perror("Error writing to file");
		close(fd);
		return 1;
	}
	close(fd);
	printf("Appened \"Goodbye!\"to\"%s\" sucessfully.\n",filename);
	return 0;
}
```
18.program to change the permissions of the file to read only.
```c
#include<stdio.h>
#include<sys/stat.h>
int main()
{
	const char *filename="destination.txt";
	if(chmod(filename,0444)==0)
	{
		printf("permissions of \"%s\" changed to read-only.\n",filename);

	}
	else
	{
		perror("error changing file permissions");
	}
	return 0;
}
```
19.Program to change the ownership of an file.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<pwd.h>
int main()
{
	const char *filename="destination.txt";
	const char *new_owner="user1";
	struct passwd *pwd=getpwnam(new_owner);
	if(pwd==NULL)
	{
		perror("getpwnan");
		return 1;
	}
	if(chown(filename,pwd->pw_uid,-1)==-1)
	{
		perror("chown");
		return 1;
	}
	printf("Owernship of '%s' changed to user'%s'.\n",filename,new_owner);
	return 0;
}
```
20.program to get the last modified timestamp of a file.
```
#include<stdio.h>
#include<sys/stat.h>
#include<time.h>
int main()
{
	const char *filename="destination.txt";
	struct stat filestat;
	if(stat(filename,&filestat)==-1)
	{
		perror("stat");
		return 11;
	}
	else
	{
		printf("last modified time of '%s': %s",filename,ctime(&filestat.st_mtime));
		return 0;
	}
}
```
21.Program to check wheter it is an file or directory.
```c
#include<stdio.h>
#include<sys/stat.h>
#include<unistd.h>
int main()
{
	char name[20];
	struct stat pathstat;
	printf("enter the name");
	scanf("%s",name);
	if(stat(name,&pathstat)!=0)
	{
		perror("stat");
		return 1;
	}
	if(S_ISREG(pathstat.st_mode))
	{
		printf("%s is regular file",name);
	}
	else if(S_ISDIR(pathstat.st_mode))
	{
		printf("%s is a directory.\n",name);
	}
	else{
		printf("%s is neither a regular file nor a directory.\n",name);
	}
	return 0;
}
```
22.program to create a link to a file to file.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	const char *original="destination.txt";
	const char *hlink="linked.txt";
	if(link(original,hlink)==0)
	{
		printf("link %s created for %s sucessfully.\n",hlink,original);
	}
	else
	{
		perror("error creating hardlink");
	}
	return 0;
}
```
23.Program to get the path of an current working directory.
```c
#include<stdio.h>
#include<unistd.h>
#include<limits.h>
int main()
{
	char cwd[PATH_MAX];
	if(getcwd(cwd,sizeof(cwd))!=NULL)
	{
		printf("current working directory: %s\n",cwd);
	}
	else{
		perror("getcwd() error");
	}
	return 0;
}
```
24.Program to create a fifo.
```c
#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
int main()
{
	const char *fifo="myfifo";
	if(mkfifo(fifo,0666)==0)
	{
		printf("%s created sucessuflly",fifo);
	}
	else
	{
		printf("Error creating FIFO");
	}
	return 0;
}
```
25.Program to print the contents present in an myfifo.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
	const char *file="myfifo";
	char buffer[1024];
	int fd,bytes;
	fd=open(file,O_RDONLY);
	if(fd<0)
	{
		printf("Error occured opening an file.");
		return 1;
	}
	printf("Waiting for data from FIFO %s \n",file);
	while((bytes=read(fd,buffer,sizeof(buffer)-1))>0)
	{
		buffer[bytes]='\0';
		printf("received: %s\n",buffer);
	}
	if(bytes<0)
	{
		perror("Error reading from FIFO");
	}
	close(fd);
	return 0;
}
```
26.Program to truncate the filenamed to an speceifed length.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	const char *filename="file.txt";
	off_t new_length;
	printf("enter the new length to truncate %s to: ",filename);
	scanf("%ld",&new_length);
	if(truncate(filename,new_length)==0)
	{
		printf("Sucessfully truncated %s to %ld bytes.\n",filename,new_length);
	}
	else{
		perror("truncate");
		return 1;
	}
	return 0;
}
```
27.Program to Search for an specific string in file and display the line number where it occurs.
```c
#include<stdio.h>
#include<string.h>
int main()
{
	FILE *file=fopen("linked.txt","r");
	if(file==NULL)
	{
		perror("Error opening file");
		return 1;
	}
	char line[1024];
	char search[100];
	int line_number=0;
	int found=0;
	printf("Enter the string to search: ");
	scanf("%s",search);
	while(fgets(line,sizeof(line),file))
	{
		line_number++;
		if(strstr(line,search)!=NULL)
		{
			printf("Found on line %d: %s",line_number,line);
			found=1;
		}
	}
	if(!found)
	{
		printf("String \"%s\" not found in file.\n",search);
	}
	fclose(file);
	return 0;
}
```
28.program to get the file type.
```c
#include<stdio.h>
#include<sys/stat.h>
#include<unistd.h>
int main()
{
	char path[256];
	struct stat path_stat;
	printf("Enter the path: ");
	scanf("%s",path);
	if(lstat(path,&path_stat)!=0)
	{
		perror("lstat");
		return 1;
	}
	if(S_ISREG(path_stat.st_mode))
		printf("%s is a regular file.\n",path);
	else if(S_ISDIR(path_stat.st_mode))
		printf("%s is a directory.\n",path);
	else if(S_ISFIFO(path_stat.st_mode))
		printf("%s is a FIFO(named pipe).\n",path);
	else
		printf("%s is an unknown type.\n",path);
	return 0;
}
```
29.Program to create an empty file.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
	const char *filename="empty.txt";
	int fd=open(filename,O_CREAT,0222);
	if(fd<0)
	{
		perror("error creating file");
		return 1;
	}
	printf("File %s created sucessfully.\n",filename);
	close(fd);
	return 0;
}
```
30.Program to create an empty file(another way).
```c
#include<stdio.h>
#include<sys/stat.h>
int main()
{
	const char *filename="empty.txt";
	struct stat st;
	if(stat(filename,&st)!=0)
	{
		perror("stat");
		return 1;
	}
	printf("Permissions: %o\n",st.st_mode &0777);
	return 0;
}
```
31.Program to get an permissions mode of an given file.
```c
#include<stdio.h>
#include<sys/stat.h>
void print_permissions(mode_t mode)
{
	//File Type
	printf((S_ISDIR(mode))? "d":"-");
	//Owner
	printf((mode & S_IRUSR)?"r":"-");
	printf((mode & S_IWUSR)?"w":"-");
	printf((mode & S_IXUSR)?"x":"-");
	//Group
	printf( (mode & S_IRGRP) ? "r" : "-");
    printf( (mode & S_IWGRP) ? "w" : "-");
    printf( (mode & S_IXGRP) ? "x" : "-");
    // Others
    printf( (mode & S_IROTH) ? "r" : "-");
    printf( (mode & S_IWOTH) ? "w" : "-");
    printf( (mode & S_IXOTH) ? "x" : "-");
}
int main()
{
	const char *filename="linked.txt";
	struct stat st;
	if(stat(filename,&st)!=0)
	{
		perror("stat");
		return 1;
	}
	printf("permission for %s: ",filename);
	print_permissions(st.st_mode);
	printf(" (octal: %o)\n",st.st_mode & 0777);
	return 0;
}
```
32.program to print the 10 lines of the file.
```c
#include<stdio.h>
int main()
{
	FILE *fp=fopen("linked.txt","r");
	if(fp==NULL)
	{
		perror("Error opening linked.txt");
		return 1;
	}
	char buffer[1024];
	int line=0;
	while(fgets(buffer,sizeof(buffer),fp)!=NULL &&line<10)
	{
		printf("%s",buffer);
		line++;
	}
	fclose(fp);
	return 0;
}
```
33.Program to read the file from another file and print in reverse order.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
	FILE *infile=fopen("linked.txt","r");
	FILE *outfile=fopen("truncate.txt","w");
	if(!infile || !outfile)
	{
		perror("Error opening file");
		return 0;
	}
	fseek(infile,0,SEEK_END);
	long size=ftell(infile);
	for(long i=size-1;i>=0;i--)
	{
		fseek(infile,i,SEEK_SET);
		int ch=fgetc(infile);
		fputc(ch,outfile);
	}
	fclose(infile);
	fclose(outfile);
	printf("Contents of written in reverse to \n");
	return 0;
}
```
34.program to create a directory with an date,month and year.
```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<sys/stat.h>
#include<sys/types.h>
int main()
{
	time_t now=time(NULL);
	struct tm *t=localtime(&now);
	char dirname[100];
	strftime(dirname,sizeof(dirname),"%Y-%m-%d",t);
	if(mkdir(dirname,0755)==0)
	{
		printf("Directory %s created sucessfully.\n",dirname);
	}
	else
	{
		perror("mkdir");
	}
	return 0;
}
```
35.program to Read and Display the contents of an binary file.
```c
#include<stdio.h>
int main()
{
	FILE *file=fopen("binary.bin","rb");
	if(file==NULL)
	{
		perror("Error opening File");
		return 1;
	}
	unsigned char buffer;
	int byte_count=0;
	printf("Contents of binary.bin in hexadecimal: \n");
	while(fread(&buffer,1,1,file)==1)
	{
		printf("%x",buffer);
		if(byte_count % 16==0)
		{
			printf("\n");
		}
	}
	if(byte_count % 16!=0)
	{
		printf("\n");
	}
	fclose(file);
	return 0;
}
```
36.Program to create a binary file and content to it.
```c
#include<stdio.h>
int main()
{
	FILE *file=fopen("binary.bin","wb");
	if(!file)
	{
		perror("Error creating binary file");
		return 1;
	}
	int numbers[]={10,20,30,40,50};
	size_t written=fwrite(numbers,sizeof(int),5,file);
	if(written!=5)
	{
		perror("Error writing data to binary file");
	}
	else
	{
		printf("Successfully wrote %u integers to binary.bin.\n",written);
	}
	fclose(file);
	return 0;
}
```
37.Program to check which is an large file.
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>
#include<sys/stat.h>
int main()
{
	const char *dirname=".";
	DIR *dir=opendir(dirname);
	if(!dir)
	{
		perror("opendir");
		return 1;
	}
	struct dirent *entry;
	struct stat file_stat;
	char path[1024];
	off_t max_size=0;
	char largest_file[1024]="";
	while((entry=readdir(dir))!=NULL)
	{
		if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
		continue;
		snprintf(path,sizeof(path),"%s/%s",dirname,entry->d_name);
		if(stat(path,&file_stat)==0 && S_ISREG(file_stat.st_mode))
		{
			if(file_stat.st_size>max_size)
			{
			max_size=file_stat.st_size;
			strncpy(largest_file,entry->d_name,sizeof(largest_file));
			}
		}
	}
	closedir(dir);
	if(strlen(largest_file)>0)
	{
		printf("Largest file: %s(%ld bytes)\n",largest_file,max_size);
	}
	else{
		printf("No regular files found in dirrectory %s \n",dirname);
	}
	return 0;
}
```
38.Program to check whether the file is readable or not.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	const char *filename="linked.txt";
	if(access(filename,R_OK)==0)
	{
		printf("the file %s is readble.\n",filename);
	}
	else
	{
		perror("File check");
	}
	return 0;
}
```
39.Program to create a new directory named and move all files with extension into it.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
	int status=system("mkdir -p Logs && mv *.log Logs/");
	if (status==0)
	{
		printf("All.log files moved to Logs directory successfully.\n");
	}
	else
	{
		printf("Something went wrong while moving.log files.\n");
	}
	return 0;
}
```
40.Program to check whether a file is writable or not.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	const char *filename="truncate.txt";
	if(access(filename,W_OK)==0)
	{
		printf("file is writeable %s",filename);
	}
	else{
		perror("not writable");
	}
	return 0;
}
```
41.program to get the number of hard links to a file "linked.txt".
```c
#include<stdio.h>
#include<sys/stat.h>
int main()
{
	const char *filename="linked.txt";
	struct stat st;
	if(stat(filename,&st)!=0)
	{
		perror("stat");
		return 1;
	}
	printf("NUmber of hardlinks to %s: %lu\n",filename,(unsigned long)st.st_nlink);
	return 0;
}
```
42.Program to combine all text files to another text file.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
	int status=system("cat *.txt>combined.txt");
	if(status==0)
	{
		printf("All.txt files merged into combined.txt sucessfully.\n");
	}
	else
	{
		printf("Failed to merge text files.\n");
	}
	return 0;
}
```
43.Program to create a new directory named with current timestamp in format "YYYY-MM-DD-HH-MM-SS".
```c
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
#include<sys/stat.h>
#include<sys/types.h>
int main()
{
	time_t now=time(NULL);
	struct tm *t=localtime(&now);
	char dirname[100];
	strftime(dirname,sizeof(dirname), "%Y-%m-%d-%H-%M-%S",t);
	if(mkdir(dirname,0755)==0)
	{
		printf("Directory %s created sucessfully.\n",dirname);
	}
	else
	{
		perror("mkdir");
	}
	return 0;
}
```
44.Program to create a directory named Document.
```c
#include<stdio.h>
#include<sys/types.h>
#include<sys/stat.h>
int main()
{
	int status=mkdir("Documents",0777);
	if(status<0)
	{
		perror("Directory creation failed.");
	}
	else{
		printf("created Sucessfully.");
	}
}
```
45.Program to check whether a file is present or not.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
	const char *filename="empty.txt";
	int status=access(filename, F_OK);
	if(status==0)
	{
		printf("File Exists: ");
	}
	else{
		printf("File does not Exists");
	}
}
```
46.Program to open a file and print it.
```c
#include<stdio.h>
int main()
{
	FILE *fp=fopen("truncate.txt","r");
	if(!fp)
	{
		perror("Error opening file");
		return 1;
	}
	char line[1024];
	while(fgets(line,sizeof(line),fp))
	{
		printf("%s",line);
	}
	fclose(fp);
	return 0;
}
```
47.Program to Copy the Contents from an one file to an another an file.
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
	FILE *dest, *src=fopen("linked.txt","r");
	char ch;
	if(!src)
	{
		perror("Error openung linked file.");
		return 1;
	}
	dest=fopen("truncate.txt","w");
	if(!dest)
	{
		perror("Error opening truncate file.");
		fclose(src);
		return 1;
	}
	while((ch=fgetc(src))!=EOF)
	{
		fputc(ch,dest);
	}
	printf("Contents copied from linked.txt to truncate.txt sucessfully.");
	fclose(src);
	fclose(dest);
	return 0;
}
```
48.program to copy the elements from an one file to an another file.(Different way)
```c
#include<stdio.h>
int main()
{
	const char *src="combined.txt";
	const char *des="test/combined.txt";
	if(rename(src,des)==0)
	{
		printf("file copied sucessfully %s %s",src,des);
	}
	else
	{
		perror("Error moving file");
	}
	return 0;
}
```
49.Program to get the size of an file named "image.jpg".
```c
#include <stdio.h>
#include <sys/stat.h>
int main() {
    const char *filename = "image.jpg";
    struct stat st;
    if (stat(filename, &st) == 0) {
        printf("Size of \"%s\": %d bytes\n", filename, (int)st.st_size);
    } else {
        perror("stat");
        return 1;
    }
    return 0;
}
```
50.Program to count the number of words present in "linked.txt".
```c
#include<stdio.h>
#include<ctype.h>
int main()
{
	FILE *vis=fopen("linked.txt","r");
	if(!vis)
	{
		perror("Error opening a file");
		return 1;
	}
	int word=0,word_count=0;
	int ch;
	while((ch=fgetc(vis))!=EOF)
	{
		if(isspace(ch))
		{
			word=0;
		}
		else if(!word)
		{
			word=1;
			word_count++;
		}
	}
	fclose(vis);
	printf("Total number of words %d:",word_count);
	return 0;
}
```
