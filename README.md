# Cn
1.import socket

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

client

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



4.import socket

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

Client:

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
