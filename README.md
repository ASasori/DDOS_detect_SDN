# DDOS detect use mininet, osken, Decision Tree

# (Link máy ảo)[] 

## Set up mininet, osken

1) update 

    ```bash
    sudo apt update && sudo apt upgrade
    sudo apt-get update
    ```
2) install git
    ```bash
    sudo apt install git
    ```
3) install mininet
    ```bash
    sudo apt-get install mininet
    ```
- check mininet version
    ```bash
    mn --version
    ```
5) set default is python3 (optional)
    ```bash
    sudo apt install python-is-python3
    ```
6) install pip 
    ```bash
    sudo apt install python3-pip
    ```
4) install os-ken
    ```bash
    git clone https://opendev.org/openstack/os-ken.git
    cd os-ken
    pip install .
    ```
- check os-ken
    ```bash
    osken-manager --version
    ```

## Thu thập dữ liệu traffic thông thường (tham khảo README.md.old)
1. Terminal 1:
    ```bash
    cd controller
    ryu-manager collect_normal_traffic.py
    ```

2. Terminal 2:
    Thay đổi địa chỉ IP controller tương ứng với thiết bị chạy controller
    ```bash
    cd mininet
    sudo python generate_normal_traffic.py
    ```

## Thu thập dữ liệu traffic DDoS (tham khảo README.md.old)
1. terminal 1:
    ```bash
    cd controller
    ryu-manager collect_ddos_traffic.py
    ```

2. Terminal 2:
    Thay đổi địa chỉ IP controller tương ứng với thiết bị chạy controller
    ```bash
    cd mininet
    sudo python generate_ddos_traffic.py
    ```

## Train model
1. Mở 1 terminal, chuyển đến thư mục machinelearning:
    ```bash
    cd machinelearning
    python DT.py
    ```

2. Load trọng số model đã train:
    ```bash
    import pickle
    with open('model.pkl', 'rb') as file:
            model = pickle.load(file)
    ```

## Phát hiện DDoS bằng Machine Learning
1. Terminal 1:
    Chạy DT_controller.py với module mitigation bằng cách import mitigation_module.py vào DT_controller.py
    ```bash
    ryu-manager DT_controller.py
    ```

2. Terminal 2:
    Thay đổi địa chỉ IP controller tương ứng với thiết bị chạy controller
    ```bash
    sudo python topology.py
    ```
3. Terminal 2:
    Trong môi trường mininet, xterm vào host tùy chọn và thực thi các file Script (đuôi .csh) như trong thư mục mininet.
