FROM debian:buster

RUN apt-get update && apt-get install -y bind9
WORKDIR /etc/bind/
RUN echo " \
zone \"chilate.occ.ues.edu.sv\" { \n \
    type master;\n \
    file \"/etc/bind/db.chilate\";\n \
    allow-transfer { 10.192.1.2; };\n \
};\n \
\n\
zone \"1.192.10.in-addr.arpa\" {\n\
    type master;\n\
    file \"/etc/bind/db.10.192.1\";\n\
    allow-transfer { 10.192.1.2; };\n\
};\n\
\n\
zone \"8.b.d.0.1.0.0.2.ip6.arpa\" {\n\
    type master;\n\
    file \"/etc/bind/db.2001.db8.1\";\n\
}; " > /etc/bind/named.conf.local \
&& echo "; BIND data file for local loopback interface\n\
;\n\
\$TTL	604800\n\
@	IN	SOA	ns1.chilate.occ.ues.edu.sv. cm16020.ues.edu.sv. (\n\
			      1		; Serial\n\
			 604800		; Refresh\n\
			  86400		; Retry\n\
			2419200		; Expire\n\
			 604800 )	; Negative Cache TTL\n\
;      \n\
;     name servers\n\
@	IN	NS	ns1.chilate.occ.ues.edu.sv.\n\
@	IN	NS	ns2.chilate.occ.ues.edu.sv.\n\
;ns1	IN	A	10.192.1.1\n\
ns1	IN	A	192.168.0.2\n\
ns2	IN	A	10.192.1.2\n\
\n\
www	IN	A	192.168.0.33\n\
;www	IN	A	10.192.1.3\n\
ww	IN	CNAME	www\n\
@	IN	AAAA	2001:db8::1" > db.chilate \
&& echo ";\n\
; BIND reverse data file for local loopback interface\n\
;\n\
\$TTL	604800\n\
@	IN	SOA	chilate.occ.ues.edu.sv cm16020.ues.edu.sv (\n\
			      1		; Serial\n\
			 604800		; Refresh\n\
			  86400		; Retry\n\
			2419200		; Expire\n\
			 604800 )	; Negative Cache TTL\n\
;\n\
@	IN	NS	ns1.chilate.occss.ues.edu.sv.\n\
@	IN	NS	ns2.chilate.occ.ues.edu.sv.\n\
1	IN	PTR	ns1.chilate.occ.ues.edu.sv.\n\
2	In	PTR	ns2.chilate.occ.ues.edu.sv.\n\
" > db.10.192.1
WORKDIR / 
RUN echo "#\!/bin/bash \\nservice bind9 start && tail -f /dev/null " > entrypoint.sh && chmod +x entrypoint.sh
ENTRYPOINT [ "sh", "/entrypoint.sh" ]  
