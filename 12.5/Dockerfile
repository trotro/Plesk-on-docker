# Plesk next version preview container

FROM ubuntu:14.04

MAINTAINER plesk-dev-leads@odin.com

ENV PLESK_DISABLE_HOSTNAME_CHECKING 1

# allow services to start
RUN sed -i -e 's/exit.*/exit 0/g' /usr/sbin/policy-rc.d

RUN apt-get update \
    && apt-get install -y wget \
    && wget -q -O /root/ai http://autoinstall.plesk.com/plesk-installer \
    && bash /root/ai \
        --select-product-id=plesk \
        --select-release-id=PLESK_12_5_30 \
        --install-component panel \
        --install-component phpgroup \
        --install-component web-hosting \
        --install-component mod_fcgid \
        --install-component proftpd \
        --install-component webservers \
        --install-component nginx \
        --install-component mysqlgroup \
        --install-component php5.6 \
        --install-component l10n \
        --install-component heavy-metal-skin \
    && plesk bin init_conf --init \
        -email changeme@example.com \
        -passwd changeme \
        -hostname-not-required \
    && plesk bin license -i A00Q00-28H603-TPKC32-NH3N93-XF3830 \
    && plesk bin settings --set admin_info_not_required=true \
    && plesk bin poweruser --off \
    && plesk bin cloning -u -prepare-public-image true -reset-license false -reset-init-conf false -skip-update true \
	&& plesk bin ipmanage --auto-remap-ip-addresses true \
	&& apt-get clean \
	&& rm -rf /root/ai /root/parallels

ADD run.sh /run.sh
CMD /run.sh

# Port to expose
# 21 - ftp
# 25 - smtp
# 53 - dns
# 80 - http
# 110 - pop3
# 143 - imaps
# 443 - https
# 3306 - mysql
# 8880 - plesk via http
# 8443 - plesk via https
EXPOSE 21 80 443 8880 8443
