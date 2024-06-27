<template>
    <ion-page>
        <ion-header>
            <ion-toolbar color="primary">
                <ion-title>Scanner</ion-title>
            </ion-toolbar>
        </ion-header>
        <ion-content :fullscreen="true">
            <ion-header collapse="condense">
                <ion-toolbar>
                    <ion-title size="large">Scanner</ion-title>
                </ion-toolbar>
            </ion-header>
            <qrcode-stream @detect="onDetect" :track="paintOutline"
                :formats="['qr_code', 'code_128', 'upc_a', 'ean_13']">
            </qrcode-stream>
        </ion-content>

    </ion-page>
</template>

<script setup lang="ts">
import { QrcodeStream, QrcodeDropZone, QrcodeCapture } from "vue-qrcode-reader";
import { useRouter } from "vue-router";
import { arrowBackOutline, save } from "ionicons/icons";

const router = useRouter()

const back = () => {
    router.back()
}

const onDetect = (data: any) => {
    console.log(data[0].rawValue)
}

function paintOutline(detectedCodes: any, ctx: any) {
    for (const detectedCode of detectedCodes) {
        const [firstPoint, ...otherPoints] = detectedCode.cornerPoints

        ctx.strokeStyle = 'red'

        ctx.beginPath()
        ctx.moveTo(firstPoint.x, firstPoint.y)
        for (const { x, y } of otherPoints) {
            ctx.lineTo(x, y)
        }
        ctx.lineTo(firstPoint.x, firstPoint.y)
        ctx.closePath()
        ctx.stroke()
    }
}
</script>