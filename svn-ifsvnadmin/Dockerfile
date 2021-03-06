FROM centos:7

ENV SVNADMIN_VERSION=1.6.2

COPY svnadmin-${SVNADMIN_VERSION}.zip svnadmin-${SVNADMIN_VERSION}.zip
RUN set -x && \
    yum -y update && \
    yum -y install httpd httpd-devel mod_dav_svn mod_ldap mod_ssl subversion php php-ldap wget vim unzip && \
    yum clean all && \
    unzip svnadmin-${SVNADMIN_VERSION}.zip && \
    cp -rf iF.SVNAdmin-stable-${SVNADMIN_VERSION} /var/www/html/svnadmin && \
    chmod -R 777 /var/www/html/svnadmin/data && \
    mkdir /etc/pki/svnadmin /var/www/svn && \
    touch /var/www/svn/svn_auth_file.conf && \
    rm -rf svnadmin-${SVNADMIN_VERSION}.zip iF.SVNAdmin-stable-${SVNADMIN_VERSION}
COPY privkey.pem server.crt /etc/pki/svnadmin/
COPY svnserve svn_user_file.conf /var/www/svn/
COPY svnadmin.conf /etc/httpd/conf.d/
COPY config.ini userroleassignments.ini /var/www/html/svnadmin/data/
RUN set -x && \
    rm -f /etc/sysconfig/svnserve && \
    ln -s /var/www/svn/svnserve /etc/sysconfig/svnserve && \
    chown -R apache:apache /var/www/html/svnadmin && \
    chown -R apache:apache /var/www/svn && \
    chown apache:apache /var/www/html/svnadmin/data/config.ini && \
    chown apache:apache /var/www/html/svnadmin/data/userroleassignments.ini && \
    chmod 644 /var/www/html/svnadmin/data/config.ini && \
    chmod 644 /var/www/html/svnadmin/data/userroleassignments.ini && \
    chmod 644 /var/www/svn/svn_auth_file.conf && \
    chmod 644 /var/www/svn/svn_user_file.conf && \
    usermod -a -G root apache

EXPOSE 80 443

ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]

