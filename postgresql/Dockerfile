FROM ubuntu
MAINTAINER shunhsiung@gmail.com

RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.5 postgresql-client-9.5 postgresql-contrib-9.5 postgresql-9.5-postgis-2.2

USER postgres

RUN /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker &&\
	echo "localhost:5432:docker:docker:docker" > ~/.pgpass &&\
	chmod 0600 ~/.pgpass &&\
	psql -h localhost -p 5432 -d docker -U docker -f /usr/share/postgresql/9.5/contrib/postgis-2.2/postgis.sql &&\
	psql -h localhost -p 5432 -d docker -U docker -f /usr/share/postgresql/9.5/contrib/postgis-2.2/spatial_ref_sys.sql &&\
	psql -h localhost -p 5432 -d docker -U docker -W -f /usr/share/postgresql/9.5/contrib/postgis-2.2/postgis_comments.sql

RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.5/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.5/main/postgresql.conf

EXPOSE 5432

VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

CMD ["/usr/lib/postgresql/9.5/bin/postgres", "-D", "/var/lib/postgresql/9.5/main", "-c", "config_file=/etc/postgresql/9.5/main/postgresql.conf"]
