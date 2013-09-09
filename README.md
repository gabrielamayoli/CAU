##1 - Licença deste Documento

Para a utilização deste documento é necessário seguir as regras da licença Creative Commons pela mesma Licença 2.5 Brasil.

Você tem a liberdade de:

![](https://raw.github.com/gabrielamayoli/CAU/master/imagens/1.png) &nbsp;  Compartilhar — copiar, distribuir e transmitir a obra.

![](https://raw.github.com/gabrielamayoli/CAU/master/imagens/2.png) &nbsp; Remixar — criar obras derivadas.

Sob as seguintes condições:

![](https://raw.github.com/gabrielamayoli/CAU/master/imagens/3.png) &nbsp; Atribuição — Você deve creditar a obra da forma especificada pelo autor ou licenciante (mas não de maneira que sugira que estes concedem qualquer aval a você ou ao seu uso da obra).

![](https://raw.github.com/gabrielamayoli/CAU/master/imagens/4.png) &nbsp; Compartilhamento pela mesma licença — Se você alterar, transformar ou criar em cima desta obra, você poderá distribuir a obra resultante apenas sob a mesma licença, ou sob uma licença similar à presente.

Ficando claro que: <b>Renúncia</b> — Qualquer das condições acima pode ser <u>renunciada</u> se você obtiver permissão do titular dos direitos autorais. 
<b>Domínio Público</b> — Onde a obra ou qualquer de seus elementos estiver em domínio público sob o direito aplicável, esta condição não é, de maneira alguma, afetada pela licença. 
<b>Outros Direitos</b> — Os seguintes direitos não são, de maneira alguma, afetados pela licença: 
Limitações e exceções aos direitos autorais ou quaisquer usos livres aplicáveis; 
Os direitos morais do autor; 
Direitos que outras pessoas podem ter sobre a obra ou sobre a utilização da obra, tais como direitos de imagem ou privacidade. 
<b>Aviso</b> — Para qualquer reutilização ou distribuição, você deve deixar claro a terceiros os termos da licença a que se encontra submetida esta obra. A melhor maneira de fazer isso é com um link para esta página. 


2 - Introdução ao Sistema CAU

A instalação do Sistema CAU é um processo bastante simples. O sistema possui duas interfaces de interação: cau e gestaoti. A interface cau é destinada a abertura e acompanhamento de chamados pelos usuários demandantes, já a interface gestaoti é destinada ao atendimento e gerenciamento das demandas e todas as atividades de configuração do sistema. Por este motivo, neste manual várias vezes nos referimos ao termo gestaoti ao invés do nome do sistema – cau. Neste manual utilizaremos o termo sisgestaoti para referenciar a raiz da aplicação, caso queira você pode utilizar outro. 

2.1 - Características e principais funcionalidades

O código fonte (que é livre e aberto) está disponível para ser baixado livremente no Portal do SPB e sua implementação é em PHP, tendo como camada de armazenamento o sistema gerenciador de banco de dados PostgreSQL. 

O sistema possui várias funcionalidades entre as quais destacam-se: 

* Gestão de Ativos de TI
    Sistemas de Informação;
    Servidores;
    Patrimônio;
    Análise de impacto (Gestão de Configuração);
 
* Gestão de Profissionais de TI e Clientes
    Cadastro dos profissionais de TI;
    Cadastro de equipes;
    Time sheet;
    Cadastro dos clientes;
 
* Gestão de Chamados a TI
    Gestão de requisições de serviço;
    Gestão de incidentes;
    Gestão de problemas;
    Gestão de níveis de serviços;
 
* Gestão de Mudanças
   Relatórios de Apoio a Decisão;


3 - Instalação do Sistema
Requisitos gerais para instalação
Sugerimos que este guia seja executado por um usuário com experiência em configuração básica de Apache, PHP e PostgreSQL.

Este roteiro está baseado no Sistema Operacional GNU/Linux Debian Lenny.

Este manual pressupõe que o servidor de aplicaçãoo Web e o banco de dados estarão instalados no mesmo servidor.

Pré-requisitos de Software
Os requisitos mínimos de software para a correta instalação do Sistema CAU são:
PHP 5.x
php5-gd
php5-pgsql
Servidor Web Apache 
PostgreSQL 8.3 ou superior
Passo-a-passo da Instalação no Sistema Operacional Linux
Instalando Apache, PHP5 e PostgreSQL

$ apt-get install apache2
$ apt-get install postgresql-8.4
$ apt-get install php5 libapache2-mod-php5 php5-gd php5-pgsql
 
Download do software
Faça o download dos arquivos do sistema antes de prosseguir. A versão atual, 1.0, está disponível em pacotes ZIP e GZip. Descompacte o pacote de sua preferência no diretório raiz do seu servidor web Apache (no Debian, geralmente o diretório raiz é /var/www). 

	$ cd /var/www
      $ unzip /caminho/cau-1.0.zip
	$ mv cau-1.0  sisgestaoti
Criação do Banco de Dados
Crie o banco de dados ao qual o CAU usará para armazenar todos os dados digitados através da interface web. Os passos descritos nessa seção irão criar: 

Um usuário gestaoti no servidor PostgreSQL com a senha de acesso 'gestaoti'; 
Um banco de dados gestaoti. 
Observação: você pode usar o nome de usuário, banco de dados e senha que desejar. Esses são apenas nomes padrões que a aplicação usa para conectar-se ao banco. 
Faça login no servidor de banco de dados PostgreSQL com o cliente psql: 
	$ su
	# su - postgres
	# psql
Alternativamente, com o sudo: 

	$ sudo -u postgres psql

Crie o usuário de banco de dados que será utilizado pelo CAU: 

	postgres=# CREATE ROLE gestaoti;
	postgres=# ALTER ROLE gestaoti WITH SUPERUSER INHERIT NOCREATEROLE 		CREATEDB LOGIN PASSWORD 'gestaoti';
Crie o banco de dados: 

	postgres=# CREATE DATABASE gestaoti WITH TEMPLATE = template0 ENCODING = 'UTF8' LC_COLLATE = 'pt_BR.utf8' LC_CTYPE = 'pt_BR.utf8'; 
	postgres=# ALTER DATABASE gestaoti OWNER TO gestaoti;
	postgres=# \q
Execute o arquivo script_gestaoti.sql que vem no cau. O diretório em que esse arquivo reside é o install. 

	$ sudo -u postgres psql -d gestaoti -f /var/www/sisgestaoti/install/script_gestaoti.sql

Edite o arquivo de configuração e conceda permissões de escrita
O CAU armazena algumas configurações necessárias para a aplicação em um arquivo chamado gestaoti_configs.inc.php (em /var/www/sisgestaoti/gestaoti/), que possui uma sintaxe bem simples de entender. Caso tenha criado o banco de dados, nome de usuário ou senha com um valor diferente de gestaoti, basta editar esse arquivo para que corresponda as suas escolhas: 

	$gestaoti_settings['db_postgres_host']='localhost'; 
	$gestaoti_settings['db_postgres_port']='5432'; 
	$gestaoti_settings['db_postgres_name']='gestaoti'; 
	$gestaoti_settings['db_postgres_user']='gestaoti'; 
	$gestaoti_settings['db_postgres_pass']='gestaoti'; 
	$gestaoti_settings['db_postgres_enconding']='LATIN1';

Depois, conceda permissões de escrita no diretório cau/anexos. Uma forma prática é dar permissão de escrita para o usuário dono do diretório e para usuários de um grupo. Nesse caso, mudaremos o grupo desses diretórios para o grupo do usuário Apache. 

	# chmod -R 775 /var/www/sisgestaoti/cau/anexos
	# chown -R www-data.www-data /var/www/sisgestaoti/
Observação: www-data é o nome do grupo Apache padrão em sistemas Debian. Em outros sistemas, esse nome pode ser httpd, apache ou _www. Substitua de acordo com o usado em seu sistema operacional. 
Edite o arquivo de configuração da biblioteca JpGraph
Configure o diretório onde estão instaladas as fontes. Edite o arquivo jpg-config.inc.php em /var/www/sisgestaoti/gestaoti/include/PHP/class. Por padrão o sistema CAU utiliza a fonte arial.ttf basta incluí-la no diretório abaixo (no caso para sistemas Debian). Esta fonte está incluída no pacote ttf-mscorefonts-installer da distribuição Debian Lenny.
	DEFINE("TTF_DIR","/usr/X11R6/lib/X11/fonts/truetype/");
Configurando o PHP
Edite o arquivo php.ini da seguinte forma: 

register_globals: altere para On
	register_globals = on



Para instalações nos sistemas operacionais windows* os usuários reportaram  também a necessidade de alterar a diretira short_open_tag, mudar de “Off” para:
	short_open_tag = on

Observação: a localização do arquivo php.ini é diferente entre os sistemas operacionais. No Debian/Ubuntu, o padrão é /etc/php5/apache2/php.ini. 
Após qualquer alteração no arquivo php.ini, reinicie seu servidor web: 
	# /etc/init.d/apache2 restart
Configuração do Servidor Web – (Passo Opcional)
Agora criaremos um virtual host no servidor. Crie um novo arquivo em /etc/apache2/sites-available/ chamado sisgestaoti.local com o seguinte conteúdo:

<VirtualHost *:80>
  ServerName sisgestaoti.local
  DocumentRoot /var/www/sisgestaoti/

  <Directory /var/www/sisgestaoti>
    AllowOverride all
    Order deny,allow
    Allow from all
  </Directory>
</VirtualHost>
Edite o arquivo /etc/hosts (no Windows esse arquivo fica em C:\WINDOWS\system32\drivers\etc\hosts) e adicione a seguinte linha: 

	127.0.0.1      sisgestaoti.local

Habilite o Virtual Host: 

	# a2ensite sisgestaoti.local

Reinicie o servidor Apache: 

	# /etc/init.d/apache2 restart

Pronto. Agora, acesse o endereço http://sisgestaoti.local em seu navegador. 

Acessando  a aplicação
Abra o navegador de sua preferência e acesse o endereço  http://localhost/sisgestaoti ou  http://sisgestaoti.local (caso tenha configurado um Virtual Host). Faça o login na aplicação utilizando o usuário administrador. O login e senha para acesso são admin e admin, respectivamente. 


Link da Licença Júridica Creative Commons
http://creativecommons.org/licenses/by-sa/2.5/br/legalcode
