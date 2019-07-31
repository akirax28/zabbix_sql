# Zabbix / Auto Backup / Gafana
## Contêm:
   - Zabbix 
   - Banco de dados postgresql 
   - backup automático
   - Grafana

## Usage
   1. Clone o repositório:
     
```sh     
    $ git clone https://gitlab.ifrr.edu.br/infra/zabbix.git
```
   2. entre na pasta do repositório e faça o deploy:
```sh
    $ Docker stack deploy -c docker-compose.yml zabbix
```

## Config
   - Edit docker-compose.yml

## Backup
   - Uso
   O Backup é realizado automaticamente nos períodos definidos no arquivo docker-compose.yml
   
   - Restauração
   
   1. Apague deploy do zabbix e volume com o banco corrompido;
```sh
      $ docker stack rm zabbix
      $ docker volume ls
      $ docker volume rm "nome do volume"
```
   2. IMPORTANTE: Deve-se isola o conteiner do postgree removendo todos os conteiners que tenha ligação com o conteiner do postgre ou criando colocando # nas linhas do docker-compose.yml onde a link com o conteiner do postgre e no network do partgre;
   3. faça o deploy novamente do conteiner:
```sh
      $ Docker stack deploy -c docker-compose.yml zabbix
```
   4. procure o ID do conteiner do postgre com o comando:
  ```sh
      $ docker ps
```
   5. entre no conteiner com o comando:
```sh
      $ docker exec -it "id do canteiner" bash
```
   6. apague o banco:
```sh
      $ dropdb -U zabbix zabbix
```
   7. crie um novo banco de dados:
```sh
      $ createdb -U zabbix zabbix
```  
   8. restaure o banco de dados:
      na configuração padão do docker-compose.yml, está mapeia o volume do backup (compactados) diarios, semanais e mensais na '/backup' do conteiner do postgree;
      
```sh
      $ cd /backup/daily
      $ gunzip zabbix-20190721-000000.sql.gz
      $ psql -U zabbix zabbix < "/backup/daily/zabbix-20190721.sql"
```  
   9. apague novamente o deploy do zabbix:
```sh
      $ docker stack rm zabbix
```   
   10. retire # que foi colocada no arquivo docker-compose.yml e execute novamente o deploy.
