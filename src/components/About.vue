<template>
  <div class="about">
    <div class="profile-section">
      <label class="about-label">
        <strong>Developer:</strong> Ameesh Mohammed Pv
      </label>
      <label class="about-label">
        <strong>GitHub:</strong>
        <a href="https://github.com/4meesh" target="_blank">github.com/4meesh</a>
      </label>
      <div class="about-bio">
        <p>
          Currently learning full-stack development, advanced front-end design, backend architecture, and scalable web applications. Interested in web development, open-source contributions, cloud computing, and solving real-world problems with code. My goals are to master full-stack development, build scalable applications, and contribute to impactful open-source projects. Hobbies include building web apps, exploring new frameworks, gaming, and staying updated with the latest tech trends.
        </p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'About',
};
</script>

<style scoped>
.about .about-label {
  font-size: 14px;
  margin-bottom: 30px;
  display: block;
}
.profile-section {
  margin-bottom: 30px;
}
.about-bio {
  font-size: 13px;
  margin-bottom: 10px;
  color: #444;
}
</style>
