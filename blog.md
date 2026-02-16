# Blog

Hier versuche ich ab und zu ein paar blog-ähnliche Beiträge zu veröffentlichen – rund um Softwareentwicklung, AR/VR, KI und alles dazwischen. Es soll kein klassischer Tech-Blog sein, sondern eher eine Sammlung von Gedanken, Erfahrungen und gelegentlich auch Tutorials oder Einblicke in Projekte, an denen ich gerade arbeite.

## Kubernetes (K8s)

Zurzeit arbeite ich mich intensiv in Kubernetes ein. Dafür nutze ich ein Buch und einen Kurs auf Udemy. Im Buch bin ich ungefähr bei der Hälfte, und im Kurs habe ich gerade die erste Praxisaufgabe begonnen.

Damit ich das Gelernte direkt anwenden kann, habe ich mir privat ein kleines k3s-Cluster aufgebaut. Der Control-Plane/Server läuft als Ubuntu-VM auf meinem Proxmox-Server. Auf demselben Proxmox-Host läuft zusätzlich ein Worker als VM, und als weitere Worker habe ich zwei Raspberry Pi 4 eingebunden (einmal 4 GB RAM, einmal 8 GB RAM).

Je mehr ich über Kubernetes lerne, desto mehr Projektideen kommen dazu – das ist wirklich spannend.

- **Kubernetes Buch:** ISBN 978-3836298834  
- **Udemy Kurs:** [Einstieg in Kubernetes](https://cad-schroer.udemy.com/course/einstieg-in-kubernetes/)

---

# k3s Cluster Einrichtung (Notizen)

## Setup / Nodes

- **k3s Server (Control Plane):** Proxmox VM, Ubuntu 25.04  
- **Worker (VM):** Proxmox VM, Ubuntu 25.04  
- **Worker (Raspberry Pi 4):** Raspberry Pi OS 64-bit, 4 GB RAM  
- **Worker (Raspberry Pi 4):** Raspberry Pi OS 64-bit, 8 GB RAM  

## k3s auf dem Master installieren

```bash
# Installiert k3s als reinen Server (ohne Agent auf derselben Maschine)
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="server --disable-agent" sh -
```

```bash
# Service prüfen und Logs anschauen
sudo systemctl enable --now k3s
sudo systemctl status k3s --no-pager
sudo journalctl -u k3s -b --no-pager | tail -n 200
```

Die Standard-Kubeconfig liegt unter:

- `/etc/rancher/k3s/k3s.yaml`

Wichtig: Darin steht häufig als Server-Adresse `127.0.0.1`.  
Wenn du **lokal auf der VM** arbeitest, passt das. Wenn du `kubectl` **vom Laptop** nutzen willst, musst du die Server-Adresse auf die IP/den Hostnamen der VM ändern.

## Token auslesen (für Worker Join)

```bash
# Token anzeigen (wichtig zum Hinzufügen von Workern)
sudo cat /var/lib/rancher/k3s/server/node-token
```

```bash
# Optional: in Datei speichern
sudo cat /var/lib/rancher/k3s/server/node-token | sudo tee /root/k3s-server-node-token.txt
sudo chmod 600 /root/k3s-server-node-token.txt
```

## Worker im Cluster anzeigen

```bash
sudo k3s kubectl get nodes -o wide
```

## Worker hinzufügen

```bash
curl -sfL https://get.k3s.io | K3S_URL="https://k3s-master:6443" K3S_TOKEN="XXXXX" sh -
```

---

## Raspberry Pi: cgroup / Memory-Controller Fix (Worker Issue)

Bei mir war der Fehler recht eindeutig: k3s bzw. kubelet/containerd finden keinen **cgroup v2 Memory Controller**. Auf vielen Distributionen wird inzwischen erwartet, dass die cgroup-v2-Hierarchie aktiv ist.

Lösung: Auf dem Raspberry Pi **cgroup v2 (systemd unified cgroup hierarchy)** aktivieren. Das ist meistens nur ein Kernel-Cmdline-Parameter + Reboot.

```bash
FILE=/boot/firmware/cmdline.txt   # ggf. anpassen, falls der Pfad anders ist
sudo cp "$FILE" "${FILE}.bak"

# Parameter nur hinzufügen, wenn noch nicht vorhanden
if ! grep -q "systemd.unified_cgroup_hierarchy=1" "$FILE"; then
  sudo sed -i '1s/$/ systemd.unified_cgroup_hierarchy=1/' "$FILE"
  echo "Parameter hinzugefügt in $FILE"
else
  echo "Parameter schon gesetzt"
fi

echo "Aktuelle cmdline:"
cat "$FILE"

sudo reboot
```