# TP-PageRank


In this repository is my work for the PageRank work to do on the google clouds service. 

On commence tout d'abord avec la création du Bucket avec cette commande, l'ID du projet est mentionné sur la commande : 

    gcloud storage buckets create gs://TP_pagerank --project=pagerank-365311  --location=europe-west1 --uniform-bucket-level-access

et puis on télécharge les données qu'on va utiliser :

    gsutil cp gs://public_lddm_data/page_links.en.nt.bz2 gs://tp_pagerank/ 

# 1 - PageRank with PIG.

le code source utilisé c'est dataproc.py et le lancement des commandes sur gc est le suivant :

Création du Cluster (soit manuellement sur le service google ou soit avec cette commande avec le même project ID) : 

    gcloud dataproc clusters create cluster-a35a --enable-component-gateway --region europe-west1 --zone europe-west1-c --master-machine-type n1-standard-4 --master-boot-disk-size 500 --num-workers 2 --worker-machine-type n1-standard-4 --worker-boot-disk-size 500 --image-version 2.0-debian10 --project pagerank-365311


et on lance la commande : 

    gcloud dataproc jobs submit pig --region europe-west1 --cluster cluster-a35a -f gs://tp_pagerank/dataproc.py

Après la fin du commande, on exécutera la commande suivante pour vider le dossier : 

    gsutil rm -rf gs://tp_pagerank/out


J'ai commencé avec 2 Noeuds, puis 3, 4 et 5 Noeuds à la fin.

Les résultats sont affichés dans le tableau ci-dessous : 

| Nombre des noeuds  | Temps écoulé
| ------------------ | -----------------|
|         2          | 48min            |
|         3          | 40min 47sec      |
|         4          | 35 min           |
|        5           | 33 min 27sec     |
       
         
# 2 - PageRank with PySpark 

(La tâche de PySpark serait lancé sur le même cluster, donc ce n'est pas la peine de créer un nouveau. Même chose pour les données)

le code source utilisé c'est pagerank_notype.py et le lancement des commandes sur gc est le suivant :


    gcloud dataproc jobs submit pyspark --region europe-west1 --cluster cluster-a35a gs://tp_pagerank/pagerank_notype.py  -- gs://tp_pagerank/page_links_en.nt.bz2 3



Même chose ici, j'ai commencé avec 2 Noeuds, puis 3, 4 et 5 Noeuds à la fin.

Les résultats sont affichés dans le tableau ci-dessous : 


