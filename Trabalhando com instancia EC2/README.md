Trabalhando com instancias EC2 na AWS

About
Nesta etapa, vamos explorar o Amazon EC2, um serviço popular de infraestrutura como serviço (IaaS) oferecido pela Amazon Web Services (AWS).

Technologias
EC2
Putty
Elastic Block Store (EBS)

O Desafio
1. Configuração da instância EC2
2. Conexão via SSH
3. Gerenciando o armazenamento
4. Formatando e montando o volume
5. Criação de arquivos
6. Explorando recursos:


Configuração da Instancia EC2

Dentro do ambiente da AWS, criamos a Instancia EC2 com o nome "BandaMiguel", esta instancia utiliza uma imagem "Amazon Linux 2 AMI" com um tipo t2.micro.
![NomeInstancia](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/1782bc89-73d5-4c0b-a541-eedd9fa23fe9)

Também criamos um par de chave para conexão com segurança a nossa maquina virtual via SSH.

![Imagem e tipo de instancia](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/897a8fe2-c4df-4e1a-869b-cec59cb5c25f)

Criamos um grupo de segurança nas configurações de rede, com as seguintes regras selecionadas:
![Configurações de rede](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/2157c017-31d8-4cd9-97a9-508ca70d908d)

Conexão via SSH

Após baixarmos a chave SSH na nossa maquina, a utilizamos o comando com o endereço IP público da nossa instancia EC2:

ssh -i ~/Downloads/my-key-pair.pem ec2-user@3.123.45.67(Exemplo da nossa chave).

![Acesso SSh](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/751b62e5-48ed-4b5a-b2b1-7c193a033ff8)

Gerenciando o armazenamento

Dentro do console da AWS, navegamos até "Volume", em "Elastic Block Store(EBS)" e clicamos em "Create Volume".

Aumentamos o tamanho do nosso volume para 125Gb, um ponto importante a mencionar é de que a Zona de disponibilidade do volume deve ser a mesma zona da intancia.
![CreateVolume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/3f239cda-f32b-4590-8c9e-d1424e1481a7)

![Associação de Volume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/8b32317e-4590-42ce-9c0d-3b90e76142ea)

![RecursosAnexados](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/4c701cb3-2d6c-48a3-af5c-4627bbbb0594)

Formatando e montando o volume
Conectamos à instância EC2 via SSH, listamos os dispositivos de bloco para encontrar o novo volume criado o camando utilizado foi o lsblk :

![lsblk](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/7a3df055-4c2b-4fb7-98ac-c9c842409ccc)

Apos isso, utilizamos sudo mkfs -t ext4 /dev/sdb para formatar o nosso volume, onde:
sudo -> é um comando que permite ao usuário executar um comando com privilégios de superusuário (root).
mkfs -> significa "make filesystem" (criar sistema de arquivos). É um comando utilizado para formatar uma partição ou disco.
-t ext4 -> -t, especifica o tipo de sistema de arquivos a ser criado e ext4, é um tipo de sistema de arquivos comumente usado em sistemas Linux.
dev/sdb -> é o caminho do dispositivo ou partição onde o sistema de arquivos será criado.

![Volume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/3555c857-2808-4a94-9f60-fa6c299e663b)

Com isso, criamos um ponto de montagem:
![Meuvolume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/dedf2271-7ca8-4822-a4a3-9077a5beb352)

sendo mkdir o comando que cria um novo diretório com o nome especificado.

Para montar o volume, utilizamos:

![MontandoVolume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/ccc19f7a-bf81-4f5a-bc5e-7badfbaf0853)

Criação de arquivos

Navegamos até o diretorio que foi montado:

![CdmeuVolume](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/6818c95a-2771-425c-873f-16e2e4133731)

Criamos um arquivo de texto simples denominado testfile.txt:

![testfile](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/31fdaab3-feb8-431d-9d5c-4de2f52938fd)

Explorando Recursos

Utilizamos o recurso ls -l para verificar o conteúdo do volume montado:

![ls -l](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/a603713c-a2c7-4b29-91dc-289866994ddf)

Com o df -h verificamos o espaço em disco disponível

![df-h](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/adebd092-57c0-42a3-8bba-dcab5508f8ba)

E por fim, apresentamos o conteúdo do arquivo criado:

![Cat](https://github.com/vihjoulle/Redes-e-Linux-Essentials-para-AWS/assets/73195664/331dca4e-14ef-4639-becc-e5c22613bd2e)

