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

UDP - server side
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/socket.h>
#include<sys/types.h>
#include<unistd.h>
#include<netinet/in.h>
#include<arpa/inet.h>
#define PORT 8083
#define MAX 80
int main()
{
	struct sockaddr_in servaddr,cli;
	int sockfd,len;
	char buff[MAX];
	
	sockfd=socket(AF_INET,SOCK_DGRAM,0);
	if(sockfd==-1)
	{
		printf("Scoket creation failed\n");
		exit(0);
	}
	else
	{
		printf("Socket created\n");
	}
	bzero(&servaddr,sizeof(servaddr));
	
	servaddr.sin_family=AF_INET;
	servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
	servaddr.sin_port=htons(PORT);
	
	for(;;)
	{
		bzero(buff,MAX);
		len=sizeof(servaddr);
		printf("\nEnter the message : ");
		scanf("%s",buff);
		if(strncmp("exit",buff,4)==0)
		{
			printf("Server exit..................\n");
			exit(0);
		}
		sendto(sockfd,buff,sizeof(buff),0,(struct sockaddr*)&servaddr,len);
		bzero(buff,MAX);
		recvfrom(sockfd,buff,sizeof(buff),0,(struct sockaddr*)&servaddr,&len);
		printf("Message from server : %s",buff);
	}
	close(sockfd);

}

UDP - client side
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/socket.h>
#include<sys/types.h>
#include<unistd.h>
#include<netinet/in.h>
#include<arpa/inet.h>
#define PORT 8083
#define MAX 80
int main()
{
	struct sockaddr_in servaddr,cli;
	int sockfd,len;
	char buff[MAX];
	
	sockfd=socket(AF_INET,SOCK_DGRAM,0);
	if(sockfd==-1)
	{
		printf("Scoket creation failed\n");
		exit(0);
	}
	else
	{
		printf("Socket created\n");
	}
	bzero(&servaddr,sizeof(servaddr));
	
	servaddr.sin_family=AF_INET;
	servaddr.sin_addr.s_addr=htonl(INADDR_ANY);
	servaddr.sin_port=htons(PORT);
	
	for(;;)
	{
		bzero(buff,MAX);
		len=sizeof(servaddr);
		printf("\nEnter the message : ");
		scanf("%s",buff);
		if(strncmp("exit",buff,4)==0)
		{
			printf("Server exit..................\n");
			exit(0);
		}
		sendto(sockfd,buff,sizeof(buff),0,(struct sockaddr*)&servaddr,len);
		bzero(buff,MAX);
		recvfrom(sockfd,buff,sizeof(buff),0,(struct sockaddr*)&servaddr,&len);
		printf("Message from server : %s",buff);
	}
	close(sockfd);

}

Distance Vector
#include <stdio.h>
struct node{
	unsigned dist[20];
	unsigned from[20];
	}rt[10];

int main(){
	int costmat[20][20];
	int nodes,i,j,k,count=0;
	printf("\n enter number of nodes: \t");
	scanf("%d",&nodes);
	printf("\n enter the cost matrix: \n");
	for(i=0;i<nodes;i++){
		for(j=0;j<nodes;j++)
			{
			scanf("%d", &costmat[i][j]);
			costmat[i][i]=0;
			rt[i].dist[j] =costmat[i][j];
			rt[i].from[j] =j;
			}
		}
	do{
		count=0;
		for (i=0;i<nodes;i++){
			for (j=0;j<nodes;j++){
				for(k=0;k<nodes;k++){
					if (rt[i].dist[j] > costmat[i][k] + rt[k].dist[j]){
						rt[i].dist[j]=rt[i].dist[k]+rt[k].dist[j];
						rt[i].from[j] =k;
						count++;
						}
		}}}}
	while(count!=0);

	for(i=0;i<nodes;i++){
		printf("\n\n for router %d \n",i+1);
		for(j=0;j<nodes;j++){
			printf("\t\n nodes %d via %d distance %d",j+1,rt[i].from[j]+1,rt[i].dist[j]);
		}
		}
	printf("\n\n");
}

Leaky Bucket
#include<stdio.h>
int main(){
	int inc,outg,b_size,n,store=0;
	printf("Enter the bucket size: ");
	scanf("%d",&b_size);
	printf("Enter the outgoing rate: ");
	scanf("%d",&outg);
	printf("Enter the no. of inputs: ");
	scanf("%d",&n);
	while(n!=0){
		printf("Enter the incomimg packet size: ");
		scanf("%d",&inc);
		printf("Incoming packet size: %d\n",inc);
		if(inc<=(b_size-store)){
			store+=inc;
			printf("bucket buffer size is %d out of
			%d\n",store,b_size);
		}
		else{
			printf("Dropped number of packets: %d",inc-(b_sizestore));
			store+=b_size-store;
			printf("Bucket buffer size is %d out of %d\n",store,b_size);
		}
	store=store-outg;
	printf("After outgoing,%d packets left out of %d in buffer\n",store,b_size);
	printf("\n");
	n--;
	}
}


File Transfer - server
//SERVER
#include <stdio.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include<string.h>
int main()
{
FILE *fp1;
int sd,len,nsd,client;
char sendmsg[30],recvmsg[30],s[100];
struct sockaddr_in cliaddr , servaddr;
sd = socket(AF_INET, SOCK_STREAM , 0);
servaddr.sin_family =AF_INET;
servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
servaddr.sin_port =htons(2000);
bind(sd, (struct sockaddr*)&servaddr , sizeof(servaddr));
listen(sd,5);
client =sizeof(cliaddr);
nsd = accept(sd,(struct sockaddr*)&cliaddr,&client);
recv(nsd, recvmsg , sizeof(recvmsg),0);
char f[100];
printf("destination filename: \n");
scanf("%s",f);
fp1=fopen(f,"w");
fprintf(fp1,"%s" , recvmsg);
}

File Transfer - client

#include<stdio.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <string.h>
int main()
{
FILE *fp1;
char f[100];
printf("enter file name: \n");
scanf("%s",f);
fp1 = fopen(f,"r");
int sockfd , len;
char sendmsg[30], recvmsg[30],s[100];
struct sockaddr_in cliaddr , servaddr;
sockfd = socket(AF_INET , SOCK_STREAM,0);
servaddr.sin_family =AF_INET;
servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
servaddr.sin_port =htons(2000);
connect(sockfd , (struct sockaddr*)&servaddr , sizeof(servaddr));
fscanf (fp1,"%s",s);
strcpy(sendmsg,s);
len =strlen(sendmsg);
send(sockfd,sendmsg,strlen(sendmsg)+1 ,0);
}


```
