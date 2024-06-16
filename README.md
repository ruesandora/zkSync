# zkSync

> Ayrıntılar butonuna tıklamayı unutmayın.

> Uyarı: Hiç bir RPC'ye güvenmeyin, tüm sorumluluk bireye aittir.

> Claim günü tavsiyem 3 profil açın, 1 tane kendi oluşturduğunuz, 1 tane free rpc, 1 tane varsayılan RPC.

> Tabii 1 cüzdan için konuşuyorum ben - taktir sizindir.

> Gün içersinde ufak tefek düzenlemeler olacaktır bu repo için.

#

Hetzner Cloud tarafında CPX51'yi alabilirsiniz. Cloud sunucular saatlik maliyet hesaplar. Saati 0.053€. 

Yani 5 gün çalıştırsanız 5.5€ civarı. İmkanınız varsa daha büyük paketi alın. Resmi döküman 300GB yeterli diyor ancak onun yazıldığı zaman yetiyodur, şimdi daha fazlası gerekebilir.




<details>

![image](https://github.com/ruesandora/zkSync/assets/101149671/b9726106-8b22-4a1f-8432-f9310085606f)

</details>




```console
# Sunucu güncelleme ve docker + docker-compose kurulum işlemleriyle başlayalım.
apt update && apt upgrade -y

apt install build-essential git curl gcc make jq clang protobuf-compiler pkg-config libssl-dev -y

for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```console
# Zksync era reposunu klonlayıp klasöre giriş yapıyoruz

git clone https://github.com/matter-labs/zksync-era.git
cd zksync-era/docs/guides/external-node/docker-compose-examples
```

```console
# Mainnet .yml dosyasında değişiklik yapmamız gerek çünkü rpcyi dışarı açacağız.
# Burada dikkatli olun, sunucu IP güvenmediğiniz kimseyle paylaşmayın, saldırıya açık halde.
nano mainnet-external-node-docker-compose.yml
```

<details>

<img width="688" alt="image" src="https://github.com/neuweltgeld/zkSync/assets/101174090/e4934aa4-66c0-4f8c-998d-7a48e0a23598">

</details>

```console
# Dockeri arka planda ayağa kaldırıyoruz. Kalk ulan abin geldi.
docker compose --file mainnet-external-node-docker-compose.yml up -d
```


Grafana için localhosttan yani 127.0.0.1 den publice yani 0.0.0.0 çeviriyoruz ki dışardan erişelim.
Diğer portları da resimdeki gibi ayarlayın.

```console
# Bu adımda bekleyeceğiz. 
# Logları şu şekil kontrol edebilirsiniz
docker logs docker-compose-examples-external-node-1
```

Repo hazırlanırken 331 chunk işlenmesi gerekiyordu, kurduğunuz zamana göre değişiklik gösterebilir. 

<details>

![image](https://github.com/ruesandora/zkSync/assets/101149671/6ce870c1-19e8-4035-b626-c96d06567c1a)

</details>



Loglardaki şu satır ile kaç tane chunk kaldığını görebilirsiniz.

```Saved storage logs for chunk xxx in 234.814405933s, there are xx left to process```

İşlemler yaklaşık 1-2 saat sürebilir.
Bittiğini nerden anlayacağız ?
Kolay yolu, grafana ile
Ayrıca nodumuzu grafana ile takip de edebileceğiz. 
```http://IP:3000/d/0/external-node```

<details>

![image](https://github.com/ruesandora/zkSync/assets/101149671/74149f5e-3267-4ef4-bb4c-6ea9d2a110f1)

</details>


Bu adımlar tamam 
Metamaska kendi RPCmizi eklemek için

```console
Ağ adına istediğinizi yazın
RPC http://IP:3060
Zincir kimliği 324
Para birimi ETH
Explorer https://explorer.zksync.io/
```
