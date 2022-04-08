# anon

#include<unistd.h>
#include<sys/socket.h>
#include<sys/types.h>
#include<string.h>
#include<netinet/in.h>
#include<stdio.h>
#include<stdlib.h>
main(){
	int listfd, connfd, retval;
	socklen_t clilen;
	struct sockaddr_in cliaddr, servaddr;
	listfd = socket(AF_INET, SOCK_STREAM, 0);
	if(listfd < 0){
		perror("Sock:");
		exit(1);
	}
bzero(&servaddr, sizeof(servaddr));
servaddr.sin_family = AF_INET;
servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");
servaddr.sin_port = htons(8000);
retval = bind(listfd,(struct sockaddr *) &servaddr, sizeof(servaddr));
if(retval < 0){
	perror("bind");
	exit(2);
}

listen(listfd, 5);
while(1){
	clilen = sizeof(cliaddr);
	connfd = accept(listfd, (struct sockaddr *) &cliaddr, &clilen);
	printf("Client Connected\n");
}
}


// cli
#include<unistd.h>
#include<sys/socket.h>
#include<sys/types.h>
#include<string.h>
#include<netinet/in.h>
#include<stdio.h>
#include<stdlib.h>
main(){
char *serv_ip = "127.0.0.1";
int sockfd, ret_val;
struct sockaddr_in servaddr;
sockfd = socket(AF_INET,SOCK_STREAM, 0);
bzero(&servaddr, sizeof(servaddr));
servaddr.sin_family = AF_INET;
servaddr.sin_port = htons(8000);
inet_pton(AF_INET, serv_ip, &servaddr.sin_addr);
ret_val = connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr));
if(ret_val < 0){
	perror("Connect: ");
	exit(1);
}
printf("Client establish connection with the server\n");
close(sockfd);
}
