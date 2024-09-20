# Socket-programming
Create a socket program where a server is having a list of questions and multiple clients can access those questions :</br>
# Server code</br>
import socket</br>
import threading</br>

questions = [</br>
    "What is the capital of France?",</br>
    "Who wrote 'To Kill a Mockingbird'?",</br>
    "What is the largest planet in our solar system?",</br>
    "Who painted the Mona Lisa?",</br>
    "What is the boiling point of water?",</br>
    "Who discovered penicillin?",</br>
    "What is the square root of 64?",</br>
    "What year did the Titanic sink?",</br>
    "Who developed the theory of relativity?",</br>
    "What is the longest river in the world?"</br>
]</br>

HOST = '127.0.0.1'  # Localhost</br>
PORT = 65432        # Port to listen on</br>

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)</br>
server_socket.bind((HOST, PORT))</br>
server_socket.listen()</br>

print(f"Server is listening on {HOST}:{PORT}...")</br>

def handle_client(conn, addr):</br>
    """Function to handle communication with a connected client."""</br>
    print(f"Connected by {addr}")</br>
    try:</br>
        
        while True:</br>
            data = conn.recv(1024).decode('utf-8')</br>
            if not data:</br>
                break</br>
            try:</br>
                index = int(data)  # The client sends the question index</br>
                if 0 <= index < len(questions):</br>
                    conn.sendall(questions[index].encode('utf-8'))</br>
                else:</br>
                    conn.sendall("Invalid question number.".encode('utf-8'))</br>
            except ValueError:</br>
                conn.sendall("Please enter a valid number.".encode('utf-8'))</br>
    finally:</br>
        print(f"Connection closed by {addr}")</br>
        conn.close()</br>

try:</br>
    while True:</br>
        # Accept a new connection</br>
        conn, addr = server_socket.accept()</br>
        # Start a new thread to handle the client</br>
        client_thread = threading.Thread(target=handle_client, args=(conn, addr))</br>
        client_thread.start()</br>
finally:</br>
    server_socket.close()</br>

# client code</br>
import socket</br>

HOST = '127.0.0.1'</br>
PORT = 65432  </br>      

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)</br>

client_socket.connect((HOST, PORT))</br>

print("Connected to the server. Enter a number between 0 and 9 to get a question, or 'exit' to quit.")</br>

try:</br>
    while True:</br>
        message = input("Enter question number (0-9): ")</br>
        if message.lower() == 'exit':</br>
            break</br>
        if not message.isdigit():</br>
            print("Please enter a valid number.")</br>
            continue</br>

        
        client_socket.sendall(message.encode('utf-8'))</br>

        data = client_socket.recv(1024).decode('utf-8')</br>
        print(f"Question: {data}")</br>
finally:</br>
    client_socket.close()</br>


(2) Create a server and client connection program</br>

# Server code</br>
import socket</br>
import threading</br>


HOST = '127.0.0.3'  # Localhost</br>
PORT = 12346        # Port to listen on</br>

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)</br>
server_socket.bind((HOST, PORT))</br>
server_socket.listen(5)</br>

print(f"Server is listening on {HOST}:{PORT}...")</br>

def handle_client(client_socket, addr):</br>
    """Function to handle communication with a connected client."""</br>
    print(f"Client connected from {addr}")</br>
    while True:</br>
        try:</br>
            data = client_socket.recv(1024)</br>
            if not data or data.decode('utf-8') == 'END':</br>
                break</br>
            print(f"Received from client {addr}: {data.decode('utf-8')}")</br>
            client_socket.send(bytes('Hey client', 'utf-8'))</br>
        except Exception as e:</br>
            print(f"Error handling client {addr}: {e}")</br>
            break</br>
    print(f"Client {addr} disconnected")</br>
    client_socket.close()</br>

try:</br>
    while True:</br>
        print("Server waiting for connection...")</br>
        client_socket, addr = server_socket.accept()</br>
        # Start a new thread to handle the client</br>
        client_thread = threading.Thread(target=handle_client, args=(client_socket, addr))</br>
        client_thread.start()</br>
finally:</br>
    server_socket.close()</br>

# Client code</br>
import socket</br>
client_socket=socket.socket(socket.AF_INET,socket.SOCK_STREAM)</br>
client_socket.connect(('127.0.0.3',12346))</br>
payload="hey server"</br>

try:</br>
  while True:</br>
    client_socket.send(payload.encode('utf-8'))</br>
    data=client_socket.recv(1024)</br>
    print(str(data))</br>
    more=input("Want to send more data")</br>
    if more.lower()=='y':</br>
      payload=input("Enter payload")</br>
    else:</br>
      break</br>
except KeyboardInterrupt:</br>
  print("Exited by user")</br>
client_socket.close()</br>


