# Fix broken cert

If you accidentally uploaded a cert/key that shows erros when trying to recconect to web gui (check with journalctl -f) do the following:

```rm /etc/pve/local/pveproxy-ssl.*```

```pvecm updatecerts -f```

```systemctl restart pveproxy```

You now have a new self-signed cert generated and access should be restored.

Make sure certificate chain begins with proxmox cert and ends with CA cert.