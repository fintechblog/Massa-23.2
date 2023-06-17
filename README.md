### Cập nhật Node Massa-23.2

#### 1. Update server 
```
sudo apt update && apt upgrade -y
```
#### 2. Chuẩn bị môi trường trước khi cài đặt (Install Prerequisties) 
```
sudo apt install pkg-config curl git build-essential libssl-dev libclang-dev cmake ufw screen
```
#### 3.	Cài Rusk 
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
* Config path: source $HOME/.cargo/env
*	Check rusk version: rustc --version
#### 4.	Cài đặt Nightly 
```
rustup toolchain install nightly-2023-02-27
```
#### 5. Backup wallet 
```
mkdir -v $HOME/massa_backup

cp $HOME/massa/massa-node/config/node_privkey.key $HOME/massa_backup/node_privkey.key

cp $HOME/massa/massa-client/wallet.dat $HOME/massa_backup/wallet.dat

```
#### 6. Remove Node
```
rm -rf $HOME/massa
rm -rf /etc/systemd/system/massad.service
rm -rf /etc/systemd/system/multi-user.target.wants/massad.service
```
#### 7. Download Node
```
git clone --branch testnet https://github.com/massalabs/massa.git
```
#### 8. Cài đặt Node
Tmux >> Enter vào session mới cho Node
```
cd massa/massa-node/

RUST_BACKTRACE=full cargo run --release -- -p <YOUR PASSWORD> |& tee logs.txt
```
Chờ cho Node Compile xong (khoảng 15 phút) – Để bootstrap xong thì có thể mất nửa ngày. 
Nhấn Ctrl + B và D để thoát ra khỏi màn hình Tmux
#### 9. Cài đặt Client
Tmux >> Enter vào session mới cho Client
```
cd massa/massa-client/
cargo run --release -- -p <YOUR PASSWORD>
```
Chờ cho Node Compile xong (khoảng 15 phút). Nhấn Ctrl + B và D để thoát ra khỏi màn hình Tmux
#### 10. Copy wallet cũ vào Node mới
Copy Ví và Private key từ folder backup về
```
cp $HOME/massa_backup/node_privkey.key $HOME/massa/massa-node/config/node_privkey.key 
cp $HOME/massa_backup/wallet.dat $HOME/massa/massa-client/wallet.dat
```
#### 11. Faucet và Staking
* Faucet: Copy địa chỉ ví vào Discord Faucet để lấy Faucet
* Active roll: vào lại màn hình Client gõ dòng lệnh: 
```
node_start_staking <Your Wallet address>
```
* Buy roll: gõ dòng lệnh vào Client
```
buy_rolls <address> <roll count> <fee>
```
Ví dụ: buy_rolls AU12XXXXXXXXXXXXXSqtg 1 0
* Kiểm tra Ví bị trừ tiền chưa bằng lệnh:
```
wallet_info
```
Nếu thấy đã bị trừ tiền, bạn đã Stake thành công. Đợi role active khoảng 2-4 tiếng
#### 12. Đăng ký với Massa bot
* `Đăng ký với Massa bot`
Vào Discord chat bất kỳ với Bot để lấy nội dung ở đoạn "To register, please run the command:
```
node_testnet_rewards_program_ownership_proof <<your_staking_address>> 88xxxxxxxxxxxx96
```
Paste dòng lệnh này vào Client => Client sẽ trả lại 1 dãy ký tự rất dài:
P12jLJjvBDxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxyg3SudWcBF5J1wrgj
* `Copy đoạn text trên vào Massa bot trên discord`
Nếu bot báo *"Congratulations!"* là thành công.

