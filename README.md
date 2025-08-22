# Cara Mengatasi Deployment VM Instance neo4j Gagal di Lab "Getting Started with Neo4J Enterprise on Google Cloud" (GSP1104)

Sesuaikan variabel berikut dengan konfigurasi deployment yang gagal. <br>
*Untuk melihat konfigurasi-nya, Deployment Manager > neo4j > overview > config*
```bash
PROJECT_ID="your-project-id" #sesuaikan dengen id project
ZONE="your-zone" #sesuaikan zone dengan lab
MACHINE_TYPE="e2-standard-4"
DISK_SIZE="100"
```

Deploy neo4j enterprise dari marketplace
```bash
gcloud compute instances create neo4j \
  --project=$PROJECT_ID \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --image-family=neo4j-enterprise \
  --image-project=launcher-public \
  --boot-disk-size=${DISK_SIZE}GB \
  --boot-disk-type=pd-ssd \
  --tags=neo4j
```

Buat firewall rules untuk neo4j
```bash
gcloud compute firewall-rules create neo4j-allow \
  --allow tcp:7474,tcp:7687 \
  --target-tags neo4j \
  --source-ranges=0.0.0.0/0
```

Lalu lakukan ssh ke instance vm tersebut
```bash
gcloud compute ssh neo4j --zone=$ZONE
```

Enable & Start neo4j
```bash
sudo systemctl enable neo4j
sudo systemctl start neo4j
```

Sekarang neo4j seharusnya sudah bisa dibuka pada {External_Ip":7474}, dan dapat dilakukan login dengan username default "neo4j" dan password default "neo4j"
