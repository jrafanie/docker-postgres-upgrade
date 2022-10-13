FROM registry.access.redhat.com/ubi8/ubi

RUN ARCH=$(uname -m) && \
    dnf -y remove *subscription-manager* && \
    dnf -y update && \
    dnf -y install \
      http://mirror.centos.org/centos/8-stream/BaseOS/${ARCH}/os/Packages/centos-stream-repos-8-2.el8.noarch.rpm \
      http://mirror.centos.org/centos/8-stream/BaseOS/${ARCH}/os/Packages/centos-gpg-keys-8-2.el8.noarch.rpm \
      https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
      https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm && \
    dnf -qy module disable postgresql && \
    dnf install -y postgresql10-server \
      postgresql13-server \
      langpacks-en \
      glibc-langpack-en


ENV PGBINOLD /usr/pgsql-10/bin/
ENV PGBINNEW /usr/pgsql-13/bin/

ENV PGDATAOLD /var/lib/postgresql/10/data
ENV PGDATANEW /var/lib/postgresql/13/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade /usr/local/bin/
COPY data_pg10.tar.gz /var/lib/postgresql

ENTRYPOINT ["docker-upgrade"]
