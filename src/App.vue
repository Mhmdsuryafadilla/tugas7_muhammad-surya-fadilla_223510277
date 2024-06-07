<template>
  <div class="app">
    <h1 class="title">Daftar Kegiatan</h1>
    <div class="add-activity">
      <input v-model="newActivity" type="text" placeholder="Tambahkan kegiatan baru" />
      <button class="btn-add" @click="addActivity">Tambah</button>
    </div>
    <div class="activities">
      <div v-for="activity in filteredActivities" :key="activity.id" class="activity">
        <div class="activity-content">
          <input type="checkbox" class="checkbox" v-model="activity.completed" />
          <span :class="{ 'completed-text': activity.completed }" @click="toggleCompleted(activity)">{{ activity.name }}</span>
        </div>
        <button class="btn-edit" @click="editActivity(activity)">Edit</button>
        <button class="btn-delete" @click="deleteActivity(activity)">Hapus</button>
      </div>
    </div>
    <div class="filter">
      <input type="checkbox" v-model="showIncompleteOnly" /> <label class="filter-label">Tampilkan hanya yang belum selesai</label>
    </div>
    <div v-if="showEditModal" class="edit-modal">
      <input v-model="editedActivity.name" type="text" placeholder="Nama Kegiatan" />
      <button class="btn-save" @click="saveEditedActivity">Simpan</button>
      <button class="btn-cancel" @click="cancelEditActivity">Batal</button>
    </div>
    <div class="counter">
      Belum selesai: {{ uncompletedCount }}
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const activities = ref([]);

const newActivity = ref('');
const showIncompleteOnly = ref(false);
const showEditModal = ref(false);
const editedActivity = ref(null);

const filteredActivities = computed(() => {
  if (showIncompleteOnly.value) {
    return activities.value.filter(activity => !activity.completed);
  } else {
    return activities.value;
  }
});

const uncompletedCount = computed(() => {
  return activities.value.filter(activity => !activity.completed).length;
});

const addActivity = () => {
  if (newActivity.value.trim() !== '') {
    activities.value.push({ id: activities.value.length + 1, name: newActivity.value, completed: false });
    newActivity.value = '';
  }
};

const deleteActivity = (activity) => {
  const index = activities.value.findIndex(a => a.id === activity.id);
  if (index !== -1) {
    activities.value.splice(index, 1);
  }
};

const editActivity = (activity) => {
  editedActivity.value = { ...activity };
  showEditModal.value = true;
};

const saveEditedActivity = () => {
  const index = activities.value.findIndex(a => a.id === editedActivity.value.id);
  if (index !== -1) {
    activities.value[index] = { ...editedActivity.value };
    cancelEditActivity();
  }
};

const cancelEditActivity = () => {
  editedActivity.value = null;
  showEditModal.value = false;
};

const toggleCompleted = (activity) => {
  activity.completed = !activity.completed;
};
</script>


<style scoped>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.title {
  text-align: center;
  color: #333;
}

.add-activity {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.add-activity input[type="text"] {
  flex: 1;
  padding: 8px;
  margin-right: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.btn-add {
  padding: 8px 16px;
  background-color: #28a745; /* Warna hijau */
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.activities {
  margin-top: 20px;
}

.activity {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  background-color: #0cbbf0;
  padding: 10px;
  border-radius: 5px;
}

.activity-content {
  display: flex;
  align-items: center;
}

.checkbox {
  margin-right: 8px;
}

.completed-text {
  text-decoration: line-through;
  cursor: pointer;
}

.btn-edit,
.btn-delete {
  padding: 6px 10px;
  background-color: #333;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.btn-edit {
  margin-right: 5px;
}

.filter {
  margin-top: 20px;
}

.filter-label {
  color: #333;
  font-weight: bold;
}

.edit-modal {
  margin-top: 20px;
}

.edit-modal input[type="text"] {
  margin-right: 8px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.btn-save,
.btn-cancel {
  padding: 8px 16px;
  background-color: #28a745;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-cancel {
  margin-left: 5px;
}
</style>