---
outline: deep
---

# Revue du code du TP01 de Charles Rocheleau

Le Travail pratique 1 porte sur un lecteur mp3 utilisant typescript vite+vue.

Le code suivant est une version révisée des points moins bien réussis:

## Norme de clean code de mettre les fonctions dans des scripts à part ex: utils.ts 
```ts
<script setup lang="ts">

//situé directement dans songPlayerTime.vue
function formatTime(time: number): string {
  const minutes = Math.floor(time / 60);
  const seconds = Math.floor(time % 60);
  return `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
}

</script>

```
## En se basant sur les standards de code, le composant App.vue est surchargé et ne devrait pas gérer la responsabilité de faire la requête pour chercher les chansons.
```ts
<script setup lang="ts">

//App.vue
onMounted(async () => {
  try {
    const songsResponse = await axios.get('http://localhost:3000/songs');
    songs.value = songsResponse.data;

    const artistsResponse = await axios.get('http://localhost:3000/artists');
    artists.value = artistsResponse.data;
    
    if (songs.value.length > 0) {
      selectedSong.value = songs.value[0];
      updateAudioPath(selectedSong.value);
      playedSong.value.addEventListener('loadedmetadata', onAudioLoadedMetadata);
      playedSong.value.addEventListener('timeupdate', onAudioTimeUpdate);
    }
  } catch (error) {
    isLoading.value = false;
    console.error('Error fetching data:', error);
  }
});

</script>

```
## Pour le code suivant, c'est une mauvaise utilisation de typescript, un songService aurait dû être utilisé.
```ts
<script setup lang="ts">

//App.vue

const songsResponse = await axios.get('http://localhost:3000/songs');

const artistsResponse = await axios.get('http://localhost:3000/artists');

</script>

```
## Les composants ne respectent pas les normes de nomenclatures, notamment le Camel Case. 
```ts
<script setup lang="ts">

//App.vue

<songPlayer :artists="artists" :songs="songs" :songName="selectedSong" :audioDuration="audioDuration" :currentAudioTime="currentAudioTime" :playedSong="playedSong"/>
<songInfos :songs="songs" :artists="artists" :songName="selectedSong" />
<songsList :songs="songs" @songSelected="songSelected" @shuffleSongs="shuffleSongs" @playNextSong="playNextSong" @playPreviousSong="playPreviousSong"/>

</script>

```
## L'appel a la BD pourrait être optimisé, une fonction aurait pu être utilisée pour actualiser les données. 
```ts
<script setup lang="ts">

//App.vue

onMounted(async () => {
  try {
    const songsResponse = await axios.get('http://localhost:3000/songs');
    songs.value = songsResponse.data;

    const artistsResponse = await axios.get('http://localhost:3000/artists');
    artists.value = artistsResponse.data;
    
    if (songs.value.length > 0) {
      selectedSong.value = songs.value[0];
      updateAudioPath(selectedSong.value);
      playedSong.value.addEventListener('loadedmetadata', onAudioLoadedMetadata);
      playedSong.value.addEventListener('timeupdate', onAudioTimeUpdate);
    }
  } catch (error) {
    isLoading.value = false;
    console.error('Error fetching data:', error);
  }
});

</script>

```
## Les fonctions suivantes auraient pu être évités ou grandement optimisées.
```ts
<script setup lang="ts">

//SingPlayerControls.vue

function playButtonClicked(): void {
  isPlayVisible.value = false;
  isPauseVisible.value = true;
  isStopVisible.value = true;
  props.playedSong.play();
}

function pauseButtonClicked(): void {
  isPlayVisible.value = true;
  isPauseVisible.value = false;
  isStopVisible.value = true;
  props.playedSong.pause();
}

function stopButtonClicked(): void {
  isPlayVisible.value = true;
  isPauseVisible.value = false;
  isStopVisible.value = false;
  props.playedSong.pause();
  props.playedSong.currentTime = 0;
}

</script>

```

<script setup>
import { useData } from 'vitepress'

const { site, theme, page, frontmatter } = useData()
</script>


