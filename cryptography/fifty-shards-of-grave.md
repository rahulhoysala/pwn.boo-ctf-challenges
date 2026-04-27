# Fifty Shards of Grave


> Count Hackula does not think as mortals do. His consciousness fractures into mirrored patterns, scattered across time and memory, each one hidden beneath shifting layers of distortion. What seems like chaos is, in truth, an intricate rhythm—designed to confuse intruders and protect the essence of his awakening. Few who have glimpsed these fragments have understood them, and fewer still have lived with their minds intact. Yet traces of his thought-patterns cannot be fully concealed. They surface in fleeting bursts, shards of meaning slipping through corrupted channels like whispers in a storm. If you are to unravel the mystery of his neural encryption, you must endure this fragmentation, piece together what he has broken apart, and resist the pull of the Count’s hungry intellect. Only then can the deeper structure of his design be revealed.


Users are intended to connect to the given IP address and port. While this challenge was still up, it was clown-sec.xyz, port 4000. 

My original Python script was this:

```python
import socket
import threading
import base64
import binascii
import random
import codecs

FLAG = "boo{on3_d4rk_sp00ky_n1ght_c0unt_h4ckul4_4ttemp7ed_t0_f01l_7h3_tr1ck_0r_tr34t_p4rty_0f_7h3_y0ung_gh0st5_gho5te773s_4nd_n0n_gh0st4ri3s_by_1nduc1ng_ch4os_bu7_7h3_br4v3_p0lt3rg31st5_m4d3_th31r_5p3c1al_n1gh7_succ33d!}"
OPERATIONS = ["BASE64", "REVERSE", "HEX", "XOR key '7'", "ROT13"]

def apply_ops(fragment, ops):
    data = fragment.encode()
    for op in ops:
        if op == "BASE64":
            data = base64.b64encode(data)
        elif op == "REVERSE":
            data = data[::-1]
        elif op == "HEX":
            data = binascii.hexlify(data)
        elif op == "XOR key '7'":
            data = bytes([b ^ 7 for b in data])
        elif op == "ROT13":
            data = codecs.encode(data.decode(errors="ignore"), "rot_13").encode()
    return data

def handle_client(conn, addr):
    print(f"[+] Connection from {addr}")
    step = 0
    while step < len(FLAG):
        fragment = FLAG[step:step+5]
        ops = random.sample(OPERATIONS, len(OPERATIONS))  # shuffle ops
        encoded = apply_ops(fragment, ops)
        message = f"OPERATION: {' | '.join(ops)}\nFRAGMENT: {encoded.decode(errors='ignore', errors='ignore')}\n"
        conn.sendall(message.encode())
        step += 5

        # Wait for client input before next fragment
        try:
            data = conn.recv(1024)
            if not data:
                break
        except:
            break

    conn.close()
    print(f"[-] Connection closed {addr}")

def start_server():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind(("127.0.0.1", 4000))
    server.listen(5)
    print("[*] Server running on localhost:4000")
    while True:
        conn, addr = server.accept()
        threading.Thread(target=handle_client, args=(conn, addr)).start()

if __name__ == "__main__":
    start_server()
```


This was refactored in TypeScript and converted into a Cloudflare worker. 

It was also later decided to shift the structure slightly by setting up a websockets connection for better latency:

<img width="1485" height="687" alt="image" src="https://github.com/user-attachments/assets/9679a796-5abf-48a9-87e1-6409112adf86" />




Due to the intentionally long nature of the flag, users are expected to write a Python script to connect to the socket and perform the given operations on the string they receive, in the same order, piece by piece. A small test just to confirm that its correctly assembled:

<img width="936" height="780" alt="image" src="https://github.com/user-attachments/assets/280fe12c-181e-49ed-9b60-29ed4ec8346c" />





