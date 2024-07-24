<script setup lang="ts">
import {ref} from "vue"
import CardDialogComponent from "@/components/CardDialogComponent.vue"
import {useDraggable, VueDraggable} from "vue-draggable-plus"

type SubCardDialogItemType = {
  subCardName: string,
  subCardId: number,
  subCardItems: string[]
}

type CardItemType = {
  cardName: string,
  cardId: number,
  cardItems: SubCardDialogItemType[]
}

function initCardList() {
  const tempCardItem: CardItemType = {
    cardName: 'card ',
    cardId: 0,
    cardItems: new Array<SubCardDialogItemType>
  }
  const tempSubCardItem: SubCardDialogItemType = {
    subCardName: 'Sub card ',
    subCardId: 0,
    subCardItems: new Array<string>
  }
  const list = []
  for (let i = 1; i <= 8; i++) {
    const item: CardItemType = JSON.parse(JSON.stringify(tempCardItem))
    item.cardName += `${i}`
    item.cardId += i
    for (let j = 1; j <= 8; j++) {
      const subItem: SubCardDialogItemType = JSON.parse(JSON.stringify(tempSubCardItem))
      subItem.subCardName += `${i}-${j}`
      subItem.subCardId += i * 10 + j
      for (let k = 1; k <= 4; k++) {
        subItem.subCardItems.push(`Card item ${i}-${j}-${k}`)
      }
      item.cardItems.push(subItem)
    }
    list.push(item)
  }
  return list
}

const cardList = ref<CardItemType[]>(initCardList())

const subCardDialogVisible = ref<boolean>(false)

const subCardDialogItem = ref<SubCardDialogItemType>({
  subCardId: 0,
  subCardItems: [""],
  subCardName: ""
})

function subCardDialogShow(subCard: SubCardDialogItemType) {
  subCardDialogVisible.value = true
  subCardDialogItem.value = subCard
}

function deleteCard(cardIdx: number) {
  let list = cardList.value
  delete list[cardIdx]
  cardList.value = list
  console.log(cardList.value)
}

const el = ref()
const draggableOption = {
  animation: 150,
  ghostClass: 'ghost',
  onStart() {
  },
  onUpdate() {
    printCard()
  }
}
useDraggable(el, cardList, draggableOption)

function onUpdate() {
  printCard()
}

function onAdd() {
  printCard()
}

function remove() {
  printCard()
}

function printCard() {
  // console.log(JSON.stringify(cardList.value, null, "\t"))
}
</script>

<template>
  <el-space wrap ref="el">
    <el-card v-for="(card, cardIdx) in cardList" :key="card.cardId" class="box-card" style="width: 300px;height: 400px"
             shadow="always">
      <template #header>
        <el-row justify="space-around" class="card-header">
          <span>{{ card.cardName }}</span>
          <el-button type="danger" size="small" @click="deleteCard(cardIdx)" link>删除</el-button>
        </el-row>
      </template>
      <el-scrollbar height="300px">
        <VueDraggable class="flex flex-col gap-2 p-4 w-300px h-300px m-auto bg-gray-500/5 rounded overflow-auto"
                      v-model="card.cardItems" :animation="150" ghostClass="ghost" group="people"
                      @update="onUpdate" @add="onAdd" @remove="remove">
          <el-card v-for="subCard in card.cardItems" :key="subCard.subCardId" shadow="hover"
                   @click="subCardDialogShow(subCard)">
            {{ subCard.subCardName }}
          </el-card>
        </VueDraggable>
      </el-scrollbar>
    </el-card>
  </el-space>

  <el-dialog v-model="subCardDialogVisible" title="Sub class dialog" width="350px">
    <card-dialog-component :sub-card="subCardDialogItem"/>
  </el-dialog>
</template>
<style scoped>

</style>

