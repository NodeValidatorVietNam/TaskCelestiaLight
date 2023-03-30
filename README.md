# TaskCelestiaLight

Nếu bạn đang chạy bằng TMUX:

    B1: Quay lại Log lệnh node đang chạy
    B2: Control C
    B3: Nhập lệnh chạy node thủ công có kèm nhiệm vụ:
    
    celestia light start --core.ip https://rpc-blockspacerace.pops.one --keyring.accname my_celes_key --gateway --gateway.addr localhost --gateway.port 26659 --p2p.network blockspacerace --metrics.tls=false --metrics --metrics.endpoint otel.celestia.tools:4318
    
    
Nếu bạn chạy bằng hệ thống SystemD:

    sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
    [Unit]
    Description=celestia-lightd Light Node
    After=network-online.target

    [Service]
    User=$USER
    ExecStart=/usr/local/bin/celestia light start --core.ip https://rpc-blockspacerace.pops.one --core.rpc.port 26657 --core.grpc.port 9090 --      keyring.accname my_celes_key --metrics.tls=false --metrics --metrics.endpoint otel.celestia.tools:4318 --gateway --gateway.addr localhost --gateway.port 26659 --p2p.network blockspacerace
    Restart=on-failure
    RestartSec=3
    LimitNOFILE=4096

    [Install]
    WantedBy=multi-user.target
    EOF
    
Chạy lệnh hệ thống:
  
    cat /etc/systemd/system/celestia-lightd.service
    
    sudo systemctl enable celestia-lightd
    sudo systemctl restart celestia-lightd
    
Kiểm tra logs:

    systemctl status celestia-lightd
