# network_scanner.py
A Python script to scan and list all devices connected to the network using ARP protocol.
from scapy.all import ARP, Ether, srp

def get_connected_devices():

    target_ip = "192.168.1.1/24"
    arp = ARP(pdst=target_ip)
    ether = Ether(dst="ff:ff:ff:ff:ff:ff")
    packet = ether/arp

    result = srp(packet, timeout=3, verbose=0)[0]

    devices = []
    for sent, received in result:
        devices.append({'ip': received.psrc, 'mac': received.hwsrc})

    print("Available devices in the network:")
    print("IP" + " "*18 + "MAC")
    for device in devices:
        print(f"{device['ip']:16}    {device['mac']}")

if __name__ == "__main__":
    get_connected_devices()
