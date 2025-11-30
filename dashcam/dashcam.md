[↩ Retour à l’accueil](/index)

--------------------------------------------------------------------------------

# Dashcam

### Table des matières

- [Dashcam](#dashcam)

## Dashcam

Ayant constaté à quel point les enregistrements de dashcam peuvent être utiles en cas d’accident ou de situation imprévue, j’ai décidé de concevoir ma propre solution. Pour ce faire, j’ai réutilisé une caméra IP déjà en ma possession comme base du projet.

Dans un premier temps, j’ai imprimé en 3D un boîtier sur mesure pour la caméra, puis je l’ai fixé sur le tableau de bord de la voiture, comme illustré ci-dessous :

<div style="margin-top: 20px; margin-bottom: 30px; display: flex; justify-content: center; align-items: center; gap: 10px;">
  <img src="media/overview.jpeg" alt="overview" style="height:400px; margin-right: 10px;">
</div>

La caméra est reliée à un Raspberry Pi 4, alimenté via la prise 12 V (prise « cigarette ») du véhicule et connecté à la caméra par un câble Ethernet :

<div style="margin-top: 20px; margin-bottom: 30px; display: flex; justify-content: center; align-items: center; gap: 10px;">
  <img src="media/pi.jpeg" alt="Pi" style="height:400px; margin-right: 10px;">
</div>

J’ai ensuite développé un script Python qui démarre automatiquement l’enregistrement dès que le véhicule est mis sous tension et l’arrête lorsqu’il est éteint. L’intégrité des fichiers vidéo est assurée grâce à l’utilisation judicieuse de certaines options ffmpeg (notamment frag_keyframe, empty_moov et flush_packets), ce qui évite la corruption des clips même en cas d’arrêt soudain.

Par ailleurs, le Raspberry Pi héberge un serveur web local accessible via un point d’accès Wi-Fi qu’il crée. Toute personne connectée à ce réseau peut ainsi consulter facilement les enregistrements récents depuis un navigateur. Voici un aperçu de l’interface et du point de vue de la caméra :

<div style="margin-top: 20px; margin-bottom: 30px; display: flex; justify-content: center; align-items: center; gap: 10px;">
  <img src="media/pov.PNG" alt="pov" style="height:400px; margin-right: 10px;">
</div>

<div style="margin-bottom: 30px;display: flex; justify-content: center; align-items: center; gap: 10px;">
  <img src="media/web.PNG" alt="web" style="height:400px; margin-right: 10px;">
</div>

(voir code source [git](https://github.com/ChristopherLajoie/dashcam.git))

--------------------------------------------------------------------------------

[↩ Retour à l’accueil](/index)