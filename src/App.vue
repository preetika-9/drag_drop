<template>
  <div>
    <h1 class="title">Drag & Drop in Vue js</h1>
    <div class="container text-size">
      <p class="title">Person</p>
      <p class="title">Occupation</p>
    </div>
    <div class="container">
      <div>
        <div
          v-for="(pn, index) in peopleName"
          :key="pn.key"
          :id="'person-' + pn.key"
        >
          <div class="left-container">
            <div class="box-name">
              <p>{{ pn.name }}</p>
            </div>

            <div
              class="drop-box"
              :id="'person-drop-' + pn.key"
              @dragover.prevent
              @drop="drop($event, pn, index)"
            >
              <div>
                <template
                  v-for="(d, __index) in draggedOccupation"
                  :key="__index"
                >
                  <div v-if="d.droppedKey == index" class="dragged-item">
                    <span>{{ d.data.name }}</span>
                  </div>
                </template>
              </div>

              <div
                class="dragged-item"
                v-if="
                  !draggedOccupation.find((item) => item.droppedKey === index)
                "
              >
                <span>
                  {{ index + 1 }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div>
        <div
          v-for="(o, index) in occupation"
          :key="index"
          draggable="true"
          @dragstart="dragstart($event, o)"
        >
          <div
            class="right-container"
            :style="o ? 'box-shadow: 0px, 0px, 1px, 0px green' : ''"
          >
            {{ o.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const peopleName = ref([
  {
    name: "Mary Brown",
    key: 0,
  },

  {
    name: "John Stevens",
    key: 1,
  },

  {
    name: "Alison Jones",
    key: 2,
  },

  {
    name: "Tim Smith",
    key: 3,
  },

  {
    name: "Jenny James",
    key: 4,
  },
]);

const occupation = ref([
  {
    name: "Finance",
    key: 0,
  },
  {
    name: "food",
    key: 1,
  },

  {
    name: "health",
    key: 2,
  },

  {
    name: "Kid’s Counselling",
    key: 3,
  },

  {
    name: "Organization",
    key: 4,
  },

  {
    name: "Rooms",
    key: 5,
  },

  {
    name: "Sports",
    key: 6,
  },

  {
    name: "Tips",
    key: 7,
  },
]);

const draggedOccupation = ref([]);

const dragstart = (event, item) => {
  console.log(event, item);
  event.dataTransfer.setData("text/plain", JSON.stringify(item));
};

const drop = (event, p, index) => {
  console.log(p, "pppp");
  event.preventDefault();
  if (draggedOccupation.value.find((item) => item.droppedKey === index)) {
    return;
  }

  const indexOcc = occupation.value.findIndex((item) => item == item);

  if (indexOcc != -1) {
    occupation.value.splice(indexOcc, 1);
  }
  const data = JSON.parse(event.dataTransfer.getData("text/plain"));

  draggedOccupation.value.push({ data, droppedKey: index });
};
</script>

<style>
.title {
  text-align: center;
  padding-bottom: 20px;
}
.container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.text-size {
  font-size: 24px;
}

.left-container {
  display: flex;
  padding-bottom: 4px;
  margin-right: 50px;
}

.drop-box {
  background: white;
  margin-left: 10px;
  width: 150px;
  border: 1px solid white;
}

.box-name {
  width: 100px;
}

.dragged-item {
  display: flex;
  align-items: center;
  justify-content: center;
  color: black;
}

.right-container {
  border: 2px solid grey;
  padding: 2px 1px 1px 4px;
  margin-bottom: 10px;
  cursor: move;
}
</style>
