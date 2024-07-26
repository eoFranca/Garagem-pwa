<script setup>
import { onMounted } from 'vue';
import { PassageUser } from '@passageidentity/passage-elements/passage-user';
import { useAuthStore } from '@/stores/auth';

const authStore = useAuthStore();

const getUserInfo = async () => {
  try {
    const authToken = localStorage.getItem('psg_auth_token');
    const passageUser = new PassageUser(authToken);
    const user = await passageUser.userInfo(authToken);
    if (user) {
      await authStore.setToken(authToken);
    } else {
      authStore.unsetToken();
    }
  } catch (error) {
    authStore.unsetToken();
  }
};

onMounted(() => {
  getUserInfo();
});
</script>

<template>
<h1>Bem vindo á Garagem do França</h1>
<h2>com as melhores peças e carros</h2>
</template>

<style scoped>
h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin: 1rem 0;
  padding: 0;
}


h2 {
  font-size: 1.5rem;
  color: #7f8c8d;
  margin: 0;
  padding: 0;
}

</style>