wrk -c 10000 -t 10 -d 20 http://localhost:8081 
sudo profile -F 99 -adf 20 > basic_server_folded_g
git clone https://github.com/brendangregg/FlameGraph
nix-shell -p perl
perl ./FlameGraph/flamegraph.pl --colors=java ./basic_server_folded_g > basic_server_g.svg


wrk -t 10 -c 100 'http://[fe80::e63d:1aff:fe72:f1%swissknife0]:8080'

sudo tcpdump -i swissknife0 -n

nc -v -6 fe80::e63d:1aff:fe72:f1%swissknife1 8080
