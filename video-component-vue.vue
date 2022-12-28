
<template>
    <div>
        <video id="video" autoplay muted playsinline></video>
        <button id="start" @click="startRecording()">Start Recording</button>
        <button id="stop" @click="endRecording()">Stop Recording</button>
    
    </div>
</template>
<script setup lang="ts">
import { onMounted, ref } from 'vue';
let blobs:any = [];
let stream:any, mediaRecorder:any, blob:any,video:any
let displaying = false;
let recording = false;
let seconds: any = ref(0)
async function display() {
    stream = await navigator.mediaDevices.getUserMedia({
        audio: true,
        video: true,
    });
    video.srcObject = stream;
    displaying = true;
}
let cancel:any;
// custom timer start
let secondRun = 1
seconds.value = 0
let flag = false
let flag2 = false

function incrementSeconds() {
    if (secondRun == 1) {
    seconds.value += 1
    }
    
    if (
    seconds.value == 5 ||
    seconds.value > 5
    ) {
        flag = true
        if (!flag2) {
            endRecording()
        }
    }
}
// custom timer end

async function startRecording() {
    if (displaying) {
        // Stop all tracks of current stream
        stream.getTracks().forEach((track:any) => {
            track.stop();
        });
        cancel = setInterval(incrementSeconds, 1000)
        // Create new stream in order to start the timestamp from 0:
        stream = await navigator.mediaDevices.getUserMedia({
            audio: true,
            video: true,
        });
        // Switch on controls:
        video.setAttribute('controls', true);
        video.srcObject = stream;
        mediaRecorder = new MediaRecorder(stream);
        mediaRecorder.ondataavailable = (event:any) => {
            if (event.data) {
                blobs.push(event.data);
            }
        };
        mediaRecorder.onstop = doPreview;
        // Let's receive 1 second blobs
        mediaRecorder.start(1000);
        recording = true;
    }
}
function endRecording() {
    if (recording) {
        // Let's stop capture and recording
        mediaRecorder.stop();
        stream.getTracks().forEach((track:any) => track.stop());
        recording = false;
        clearInterval(cancel)
    }
}
function doPreview() {
    if (!blobs.length) {
        return;
    }
    // Let's concatenate blobs to preview the recorded content
    blob = new Blob(blobs, { type: mediaRecorder.mimeType });
    video.srcObject = null;
    video.src = URL.createObjectURL(
        blob,
    );
}

onMounted(()=>{
    video = document.getElementById("video");
    display()
})
</script>


