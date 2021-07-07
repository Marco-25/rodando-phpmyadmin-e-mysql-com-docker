## orquestração de containers 
### OBS: --restart=always (para inciar mysql e phpmyadmin junto com docker ao ligar pc)

- passo 1:
    - criar rede network
        - docker network create backend
- passo 2:
    - criar container mysql
        - docker run --name mysql2 --network=backend -v /home/marco/mysql/conf:/etc/mysql/conf.d -v /home/marco/mysql/:/var/lib/mysql/ -v /etc/localtime:/etc/localtime:ro -e MYSQL_ROOT_PASSWORD=root -d mysql:5.6
- passo 3:
    - criar container php myadmin
        - docker run --name myadmin --network=backend -d -e PMA_HOST=mysql2 -v /etc/localtime:/etc/localtime:ro -p 8090:80 phpmyadmin/phpmyadmin:5

- fazer um dump:
    - docker exec mysql sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /home/marco/dump_mysql/all-mysql.sql

- mysql -u root -p
- pasta de configuração
    - cd /home/marco/mysql

- criar arquivo custom.cnf
    - cd cd /home/marco/mysql/conf
    - sudo nano custom.cnf
        # conteudo arquivo
        [mysqld]
        max_allowed_packet=32M

- verificar max allowed:
    - docker exec -it mysql bash
    - mysql -p -e "show variables like '%max_allowed_packet';"