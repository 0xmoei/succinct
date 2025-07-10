# Succinct Prover Guide

## Hardware Requirements:
**Minimal Setup**
* CPU: 8 cores
* Memory: 16GB+
* NVIDIA GPU >24GB vRAM (e.g., RTX 4090, L4, A10G)

---

### Software
* Supported: Ubuntu 20.04/22.04/24.04
* NVIDIA Driver: 555+
* If you are running on Windows os locally, install Ubuntu 22 WSL using this [Guide](https://github.com/0xmoei/Install-Linux-on-Windows)

---

## Rent GPU

**Recommended GPU Providers**
* **[Vast.ai](https://cloud.vast.ai/?ref_id=62897&creator_id=62897&name=Ubuntu%2022.04%20VM)**: SSH-Key needed
  * Rent **VM Ubuntu** [template](https://cloud.vast.ai/?ref_id=62897&creator_id=62897&name=Ubuntu%2022.04%20VM)
  * Refer to this [Guide](https://github.com/0xmoei/Rent-and-Config-GPU) to generate SSH-Key, Rent GPU and connect to your Vast GPU
 
Also saw people mentioning [GenesisCloud](https://id.genesiscloud.com/signin/) & [Tensordock](https://dashboard.tensordock.com/deploy) that I am not using them due to not supporting cryptocurrencies

---

## Prover Setup
* 1- Create a Prover in [Succinct Staking Dashboard](https://staking.sepolia.succinct.xyz/prover) on Sepolia network
* 2- Save your prover 0xaddress under *My Prover* (Keep a very low amount in your wallet)
* 3- Stake $PROVE token on your Prover [here](https://staking.sepolia.succinct.xyz/)
* 4- You can add a new signer wallet (fresh wallet) in [prover interface](https://staking.sepolia.succinct.xyz/prover) to your prover since you have to input the privatekey into the CLI


* Note: I'm currently proving with less than 1000 $PROVE tokens staked while team says you need 1000 tokens, I'm experimenting things and will update this.

---

## Dependecies
### Update Packages
```
sudo apt update && sudo apt upgrade -y
sudo apt install git -y
```

### Clone Repo
```
git clone https://github.com/0xmoei/succinct

cd succinct
```

### Install Dependecies
For GPU Setups:
```
chmod +x setup.sh
sudo ./setup.sh
```

For CPU-only Setups:
Follow the [CPU Setup](https://github.com/0xmoei/succinct/blob/main/CPU-Setup.md)

---

## Setup Prover
Copy .env from example
```bash
cp .env.example .env
```
Edit `.env`:
```
nano .env
```
* Replace the variables with your own values.
* `PGUS_PER_SECOND` & `PROVE_PER_BPGU`: Keep default values, or go through [Calibrate](#calibrate-prover) section to configure them.

Unset any conflicting shell variables:
```
unset PGUS_PER_SECOND PROVE_PER_BPGU PROVER_ADDRESS PRIVATE_KEY
```

---

## Run Prover
### For GPU Setup
```console
# Ensure you are in succinct directory
cd succinct

# Run
docker compose up -d
```

### For CPU-only Setup
```console
# Ensure you are in succinct directory
cd succinct

# Run
docker compose -f docker-compose-cpu.yml up -d
```

---

## Prover Logs

### For GPU Setup
```console
# Logs
docker compose logs -f

# Last 100 logs
docker compose logs -fn 100
```

### For CPU Setup
```console
# All logs
docker compose -f docker-compose-cpu.yml logs -f

# Last 100 logs
docker compose -f docker-compose-cpu.yml logs -fn 100
```

When looking for a request:

![image](https://github.com/user-attachments/assets/9945cdd4-0b99-4dfd-ad75-ad156bc6410e)


When proving a request:

![image](https://github.com/user-attachments/assets/596d1c1a-213c-4d71-8585-58a2e5439f92)


---

## Stop Prover
### For GPU Setup
```console
docker stop sp1-gpu succinct-spn-node-1
docker rm sp1-gpu succinct-spn-node-1
```

### For CPU Setup
```console
docker stop sp1-gpu succinct-spn-node-1
docker rm sp1-gpu succinct-spn-node-1
```


---

### Common Errors
> 1- docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

* Make sure your server runs Ubuntu VM.


> 2- ERROR:  Permanent error encountered when Bid: request is not in the requested state (Client specified an invalid argument)
>
> 3- ERROR  Permanent error encountered when Bid: Timeout expired (The operation was cancelled)

* Normal errors due to network or your prover's behaviour.

---

I will update the guide with more optimization soon.
