```
TCP server side

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#define PORT 8083
#define MAX 80

int main(){
	struct sockaddr_in servadd,client;
	int sockfd,bind1,listen1,client_size,connfd;
	char buff[MAX],temp[MAX];

	sockfd = socket(AF_INET,SOCK_STREAM,0);   // Socket Creation
	if(sockfd==-1)
	{
		printf("Socket connection failed\n");
	}
	else
	{
		printf("Socket created\n");
	}

	bzero(&servaddr,sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.ssin_port = htons(PORT);

	bind1 = bind(sockfd , (struct sockaddr*)&servaddr , sizeof(servaddr));  // Binding 
	if(bind1==-1)
	{
		printf("Bind connection failed\n");
	}
	else
	{
		printf("Bind created\n");
	}

	listen1 = listen(sockfd,5);
	if(listen1==-1)
	{
		printf("listen connection failed\n");
	}
	else
	{
		printf("listen created\n");
	}

	client_size = sizeof(client); 
	connfd = listen(sockfd, (struct sockaddr*)&client,&client_size);  // Listen
	if(connfd==-1)
	{
		printf("accept connection failed\n");
	}
	else
	{
		printf("accept created\n");
	}

	for(;;){
		bzero(buff,MAX);
		read(connfd,buff,sizeof(buff));
		strcpy(temp,buff);
		len=strlen(buff);
		buff[len]='\0';
		flag=1;
		for(i=0;i<len/2;i++)
		{
			if(buff[i]!=buff[len-i-1])
			{
				flag=0;
				break;
			}
		}
		bzero(buff,MAX);
		if(flag==1)
		{
			strcpy(buff,"Palindrome");
		}
		else
		{
			strcpy(buff,"Not Palindrome");
		}
		write(connfd,buff,sizeof(buff));
		if(strncmp("exit",exi,4) == 0)
			{
				printf("server exited");
				break;
			}
	}
	close(sockfd);
}	


TCP client side

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <unistd.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#define PORT 8083
#define MAX 80

int main(){
	struct sockaddr_in servadd,client;
	int sockfd,c,;
	char buff[MAX],temp[MAX];

	sockfd = socket(AF_INET,SOCK_STREAM,0);   // Socket Creation
	if(sockfd==-1)
	{
		printf("Socket connection failed\n");
	}
	else
	{
		printf("Socket created\n");
	}

	bzero(&servaddr,sizeof(servaddr));
	servaddr.sin_family = AF_INET;
	servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
	servaddr.ssin_port = htons(PORT);

	c=connect(sockfd,(struct sockaddr*)&servaddr,sizeof(servaddr));
	if(c!=0)
	{
		printf("Connection failed\n");
	}
	else
	{
		printf("Connection created");
	}

	for(;;){
		bzero(buff,MAX);
		printf("\nEnter a string: ");
		scanf("%s",buff);
		strcpy(temp,buff);
		write(sockfd,buff,sizeof(buff));
		bzero(buff,MAX);
		read(sockfd,buff,sizeof(buff));
		printf("from server : %s",buff);
		if(strncmp("exit",exi,4) == 0)
			{
				printf("server exited\n");
				break;
			}
	}
	close(sockfd);
}

```
