# Cn
3)a(import socket
--------------------
server=socket.socket(socket.AF_INET,socket.SOCK_STREAM)

server_address=('localhost',10000)

server.bind(server_address)

server.listen(1)

connection, client_address=server.accept()

print("Connection established with: ",client_address)

data=connection.recv(1000)

print("Received:",data)

connection.sendall(data)

connection.close()

server.close()

client--------------------

import socket

client=socket.socket(socket.AF_INET,socket.SOCK_STREAM)

server_address=('localhost',10000)

client.connect(server_address)

print("Enter the message")

message=input()

print("Sending",message)

client.sendall(message.encode())

print("ORIGINAL: ",message)

data=client.recv(1000).decode()

print("ECHO: ",data)

client.close()



3)b).import socket

server=socket.socket(socket.AF_INET,socket.SOCK_STREAM)

server_address=('localhost',10000)

server.bind(server_address)

server.listen(1)

print(“waiting for connection”)

connection, client_address=server.accept()
while message!="end":

data=connection.recv(1000).decode()

if data:

print(data)

message=input()

connection.sendall(message.encode())

else:

break

connection.close()

server.close()

Client:------------------

import socket

client=socket.socket(socket.AF_INET,socket.SOCK_STREAM)

server_address=('localhost',10000)

client.connect(server_address)

message=input()

client.sendall(message.encode())

while message!="end":

data=client.recv(1000).decode()

if data:

print(data)

message=input()

client.sendall(message.encode())

else:

break

client.close()


7.import-------------------------------

java.util.*; public

class leaky

{

static int min(int x,int y)

{

if(x<y)

return x;

else

return y;

}

public static void main(String[] args)

{ int drop=0,mini,nsec,cap,count=0,i,process;

int inp[]=new int[25];

Scanner sc=new Scanner(System.in);

System.out.println("Enter The Bucket

Size\n");cap= sc.nextInt();

System.out.println("Enter The Operation

Rate\n");process= sc.nextInt();

System.out.println("Enter The No. Of Seconds You Want To

Stimulate\n");nsec=sc.nextInt();

for(i=0;i<nsec;i++)

{ System.out.print("Enter The Size Of The Packet Entering At "+ i+1 +

"sec");inp[i] = sc.nextInt();

}

System.out.println("\nSecond | Packet Recieved | Packet Sent | Packet Left |Packet Dropped|\n");

System.out.println(" \n");

for(i=0;i<nsec;i++)

{

count+=inp[i];

if(count>cap)

{ drop=count-

cap;count=cap:
}

System.out.print(i+1);

System.out.print("\t\t"+inp[i]

); mini=min(count,process);

System.out.print("\t\t"+mini);

count=count-mini;

System.out.print("\t\t"+count)

;

System.out.print("\t\t"+drop);

drop=0;

System.out.println();

}

for(;count!=0;i++)

{

if(count>cap)

{

drop=count-cap;

count=cap;

}

System.out.print(i+1);

System.out.print("\t\t0")

;

mini=min(count,process

);

System.out.print("\t\t"+

mini); count=count-

mini;

System.out.print("\t\t"+

count);

System.out.print("\t\t"+

drop);

System.out.println();

}

}

}



9.def bfe(c,intermediate_node,neighbor,no_node):------++-----------

flag=0

for i in neighbor:

for j in range(no_node):

if((c[i][intermediate_node]+c[intermediate_node][j])<

c[i][j]):

#Using Bellman Ford Equation

c[i][j]=c[i][intermediate_node]+c[intermediate_node][j]

flag=1

#c[i][i]=0

if(flag==1)

bfe(c,i,neighbor_set[i],no_node)

if(flag==0 and intermediate_node<no_node-1
):
intermediate_node+=1
bfe(c,intermediate_node,neighbor_set[intermediate_node],no_node)

return

no_node=int(input("Enter the number of nodes:"))

c=[[0 for j in range(no_node)] for i in

range(no_node)]for i in range(no_node):

print("Enter initial cost for node ",i,":

")for j in range(no_node):

c[i][j]=int(input())

print("Initial cost matrix

:\n")for i in

range(no_node):

for j in range(no_node):

print(c[i][j],end=" ")

print("\n")

neighbor_set=[[] for i in

range(no_node)]for i in range(no_node):

for j in range(no_node):

if(i!=j): #always reachable from its own

nodeif(c[i][j]!=1000):

neighbor_set[i].append(j)

print("Neighbors : \n",neighbor_set,"\n")

bfe(c,0,neighbor_set[0],no_node) #initial

callprint("final cost matrix :\n")

for i in range(no_node):

for j in range(no_node):

print(c[i][j],end="

")print("\n")

#no_node-number of nodes

#c-cost matrix

#nodeb-nodeeighbors

#1000 considered as infinity


pradeep )-------------
public class FileServer 

{ 

 public static void main(String[] args) throws Exception 

{

 //Initialize Sockets

 ServerSocket ssock = new ServerSocket(5000);

 Socket socket = ssock.accept();

 //The InetAddress specification

 InetAddress IA = InetAddress.getByName("localhost"); 

 

 //Specify the file

 File file = new File("e:\\Bookmarks.html");

 FileInputStream fis = new FileInputStream(file);

 BufferedInputStream bis = new BufferedInputStream(fis); 

 //Get socket's output stream

 OutputStream os = socket.getOutputStream();

 //Read File Contents into contents array 

 byte[] contents;

 long fileLength = file.length(); 

 long current = 0;

 long start = System.nanoTime();

 while(current!=fileLength){ 

 int size = 10000;

 if(fileLength - current >= size)

 current += size; 

 else{ 

 size = (int)(fileLength - current); 

 current = fileLength;

 } 

 contents = new byte[size]; 

 bis.read(contents, 0, size); 

 os.write(contents);

 System.out.print("Sending file ... "+(current*100)/fileLength+"% complete!");

 } 

 os.flush(); 

 //File transfer done. Close the socket connection!

 socket.close();

 ssock.close();

 System.out.println("File sent succesfully!");

 } }

 public class FileServer ---------------------

{ 

 public static void main(String[] args) throws Exception 

{

 //Initialize Sockets

 ServerSocket ssock = new ServerSocket(5000);

 Socket socket = ssock.accept();

 //The InetAddress specification

 InetAddress IA = InetAddress.getByName("localhost"); 

 

 //Specify the file

 File file = new File("e:\\Bookmarks.html");

 FileInputStream fis = new FileInputStream(file);

 BufferedInputStream bis = new BufferedInputStream(fis); 

 //Get socket's output stream

 OutputStream os = socket.getOutputStream();

 //Read File Contents into contents array 

 byte[] contents;

 long fileLength = file.length(); 

 long current = 0;

 long start = System.nanoTime();

 while(current!=fileLength){ 

 int size = 10000;

 if(fileLength - current >= size)

 current += size; 

 else{ 

 size = (int)(fileLength - current); 

 current = fileLength;

 } 

 contents = new byte[size]; 

 bis.read(contents, 0, size); 

 os.write(contents);

 System.out.print("Sending file ... "+(current*100)/fileLength+"% complete!");

 } 

 os.flush(); 

 //File transfer done. Close the socket connection!

 socket.close();

 ssock.close();

 System.out.println("File sent succesfully!");

 } }
