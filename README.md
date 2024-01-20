import platform
import socket
import subprocess
import re
import psutil
import speedtest


def get_installed_software():
    if platform.system() == "Windows":
        installed_software = subprocess.check_output(['wmic', 'product', 'get', 'name']).decode('utf-8')
        return re.findall(r'\bName\b\s*([^\r\n]*)', installed_software)
    elif platform.system() == "Linux":
        installed_software = subprocess.check_output(['dpkg', '--get-selections']).decode('utf-8')
        return re.findall(r'(\S+)(?:\s+install)', installed_software)
    else:
        return []

def get_internet_speed():
    st = speedtest.Speedtest()
    download_speed = st.download() / 10**6  # Convert to Mbps
    upload_speed = st.upload() / 10**6  # Convert to Mbps
    return f"Download Speed: {download_speed:.2f} Mbps, Upload Speed: {upload_speed:.2f} Mbps"


def get_screen_resolution():
    if platform.system() == "Windows":
        import ctypes
        user32 = ctypes.windll.user32
        return user32.GetSystemMetrics(0), user32.GetSystemMetrics(1)
    elif platform.system() == "Linux":
        # Linux-specific code
        return "Not implemented without external libraries"
    else:
        return "Not implemented for this platform"

def get_cpu_info():
    cpu_info = platform.processor()
    return cpu_info

def get_cpu_cores_threads():
    if platform.system() == "Windows":
        import multiprocessing
        return multiprocessing.cpu_count()
    elif platform.system() == "Linux":
        # Linux-specific code
        return "Not implemented without external libraries"
    else:
        return "Not implemented for this platform"

def get_gpu_model():
    # GPU information is not directly available in psutil.
    # You may need to use additional libraries or platform-specific commands.
    return "Not implemented with psutil"


def get_ram_size():
    ram_info = subprocess.check_output(['wmic', 'memorychip', 'get', 'capacity']).decode('utf-8')
    ram_size_bytes = sum(int(capacity) for capacity in re.findall(r'\bCapacity\b\s*([^\r\n]*)', ram_info))
    ram_size_gb = ram_size_bytes / (1024 ** 3)
    return ram_size_gb

def get_screen_size():
    # Screen size information is not directly available in psutil.
    # You may need to use additional libraries or platform-specific commands.
    return "Not implemented with psutil"



def get_mac_address():
    mac_address = ':'.join(re.findall('..', '%012x' % uuid.getnode()))
    return mac_address

def get_public_ip_address():
    public_ip = socket.gethostbyname(socket.gethostname())
    return public_ip

def get_windows_version():
    windows_version = platform.system() + " " + platform.version()
    return windows_version

# Displaying the information
print("Installed Software:", get_installed_software())
print("Internet Speed:", get_internet_speed())
print("Screen Resolution:", get_screen_resolution())
print("CPU Model:", get_cpu_info())
print("Number of CPU Cores and Threads:", get_cpu_cores_threads())
print("GPU Model:", get_gpu_model())
print("RAM Size (GB):", get_ram_size())
print("Screen Size:", get_screen_size())
print("MAC Address:", get_mac_address())
print("Public IP Address:", get_public_ip_address())
print("Windows Version:", get_windows_version())
# code
