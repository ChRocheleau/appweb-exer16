---
outline: deep
---

# Revue du code du TP01 de Adam Royer

Le Travail pratique 1 porte sur un lecteur mp3 utilisant typescript vite+vue.

Le code suivant est une version révisée des points moins bien réussis:

## En se basant sur les standards de code, le composant App.vue est surchargé et ne devrait pas gérer la responsabilité de jouer la chanson.
```ts
<script setup lang="ts">

//App.vue
function handleUpdateSongAudio(play: boolean) {
  if (updatedSongAudio.value) {
    if (play) {
      updatedSongAudio.value?.play()
      setInterval(() => {
        if (updatedSongAudio.value) {
          updatedSongCurrentTime.value = formatDuration(
            updatedSongAudio.value.currentTime
          )
        }
      }, 1000)
    } else {
      updatedSongAudio.value?.pause()
    }
  } else {
    if (!hasChosenSong.value) handleErrorNoSongSelected()
  }
}

```
## La fonction SetInterval() qui sert à calculer le currentTime manque de performance et pourrait-être optimisée.
```ts 
<script setup lang="ts">

//App.vue
function handleUpdateSongAudio(play: boolean) {
      setInterval(() => {
        if (updatedSongAudio.value) {
          updatedSongCurrentTime.value = formatDuration(
            updatedSongAudio.value.currentTime
          )
        }
      }, 1000)
}

```
## Pour les fonctions suivantes il y a un manquement au clean code, une répétition aurait pu être évitée.
```ts
<script setup lang="ts">

//SongsList.vue
function previousSong(): void {
  if (currentSong.value) {
    let previousSongIndex: number
    if (parseInt(currentSong.value.id) == 1)
      previousSongIndex = songs.length - 1
    else previousSongIndex = parseInt(currentSong.value.id) - 2
    const newSong = songs[previousSongIndex]
    currentSong.value = newSong
    const artist = getArtist(newSong.artistId)
    emit('update', newSong, artist)
  } else {
    emit('errorNoSongSelected')
  }
}

function nextSong(): void {
  if (currentSong.value) {
    let nextSongIndex: number
    if (parseInt(currentSong.value.id) == songs.length) nextSongIndex = 0
    else nextSongIndex = parseInt(currentSong.value.id)
    const newSong = songs[nextSongIndex]
    currentSong.value = newSong
    const artist = getArtist(newSong.artistId)
    emit('update', newSong, artist)
  } else {
    emit('errorNoSongSelected')
  }
}

```
## La fonction suivante n'est pas nécessaire et manque d'optimisation. 
```ts
<script setup lang="ts">

//App.vue
function handleErrorNoSongSelected() {
  showAlert(NO_SONG_SELECTED_ERROR_MESSAGE)
}

```
## L'indentation de la fonction suivante est mauvaise. 
```ts
<script setup lang="ts">

//utils.ts
export function formatDuration(seconds: number): string {

    const minutes: number = Math.floor(seconds / 60);
    const remainingSeconds: number = Math.floor(seconds % 60);
    const minutesString: string = minutes < 10 ? '0' + minutes.toString() : minutes.toString();
    const secondsString: string = remainingSeconds < 10 ? '0' + remainingSeconds.toString() : remainingSeconds.toString();
    return ${minutesString}:${secondsString};
  }

```
## La fonction suivante ne gère pas les erreurs face aux appels de l'API.
```ts
<script setup lang="ts">

//SongList.vue
function randomSong(): void {
  const randomIndex = Math.floor(Math.random() * songs.length)
  const song = songs[randomIndex]
  currentSong.value = song
  const artist = getArtist(song.artistId)
  emit('update', song, artist)
}

```

<script setup>
import { useData } from 'vitepress'

const { site, theme, page, frontmatter } = useData()
</script>