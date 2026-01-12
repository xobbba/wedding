<template>
  <q-page>
    <MainComponent/>
    <InviteComponent/>
    <PlaceComponent/>
    <TimingComponent/>
    <DressCodeComponent/>
    <FormComponent/>
    <ChatComponent/>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import MainComponent from 'src/components/wedding/main/index.vue'
import InviteComponent from 'src/components/wedding/invite/index.vue'
import PlaceComponent from 'src/components/wedding/place/index.vue'
import TimingComponent from 'src/components/wedding/timing/index.vue'
import DressCodeComponent from 'src/components/wedding/dressCode/index.vue'
import FormComponent from 'src/components/wedding/form/index.vue'
import ChatComponent from 'src/components/wedding/chat/index.vue'

export default defineComponent({
  name: 'indexPage',
  components: { MainComponent, InviteComponent, PlaceComponent, TimingComponent, DressCodeComponent, FormComponent, ChatComponent }
})
</script>
