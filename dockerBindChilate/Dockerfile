FROM debian:buster

RUN apt-get update && apt-get install -y bind9
WORKDIR /etc/bind/
RUN echo " \
zone \"chilate.occ.ues.edu.sv\" { \n \
    type master;\n \
    file \"/etc/bind/db.chilate\";\n \
    allow-transfer { IPNS2; };\n \
};\n \
\n\
zone \"1.192.10.in-addr.arpa\" {\n\
    type master;\n\
    file \"/etc/bind/db.10.192.1\";\n\
    allow-transfer { IPNS2; };\n\
};\n\
\n\
zone \"chilaquila.occ.ues.edu.sv\" { \n \
    type slave;\n \
    file \"db.chilaquila\";\n \
    masters { IPNS2; };\n \
};\n \
\n\
zone \"25.192.10.in-addr.arpa\" {\n\
    type slave;\n\
    file \"db.10.192.25\";\n\
    masters { IPNS2; };\n\
};" > /etc/bind/named.conf.local \
&& echo "; BIND data file for local loopback interface\n\
;\n\
\$TTL	172800\n\
@	IN	SOA	ns1.chilate.occ.ues.edu.sv. cm16020.ues.edu.sv. (\n\
			      1		; Serial\n\
			     2d		; Refresh\n\
			  86400		; Retry\n\
			2419200		; Expire\n\
			     1d )	; Negative Cache TTL\n\
;      \n\
;     name servers\n\
@	IN	NS	ns1.chilate.occ.ues.edu.sv.\n\
@	IN	NS	ns2.chilate.occ.ues.edu.sv.\n\
ns1	IN	A	IPNS1\n\
ns2	IN	A	IPNS2\n\
www	IN	A	10.192.25.3\n\
ww	IN	CNAME	www " > db.chilate \
&& echo "; BIND reverse data file for local loopback interface\n\
;\n\
\$TTL	604800\n\
@	IN	SOA	chilate.occ.ues.edu.sv cm16020.ues.edu.sv (\n\
			      1		; Serial\n\
			 604800		; Refresh\n\
			  86400		; Retry\n\
			2419200		; Expire\n\
			 604800 )	; Negative Cache TTL\n\
;\n\
@	IN	NS	ns1.chilate.occ.ues.edu.sv.\n\
@	IN	NS	ns2.chilate.occ.ues.edu.sv.\n\
3	IN	PTR	www.chilate.occ.ues.edu.sv.\n\
" > db.10.192.1 && \
sed -i '13i\        recursion yes; \ allow-recursion { any; };' named.conf.options 
WORKDIR / 
RUN echo "#\!/bin/bash \n\
echo \$IPNS1 && echo \${IPNS1} && \
printenv && \
sed -i -e \"s/IPNS2/\$IPNS2/; s/IPNS1/\$IPNS1/\" /etc/bind/named.conf.local && \
sed -i -e \"s/IPNS2/\$IPNS2/; s/IPNS1/\$IPNS1/\" /etc/bind/db.chilate && \
named-checkconf -z && \
service bind9 start && \
tail -f /dev/null " > entrypoint.sh && chmod +x entrypoint.sh
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]   