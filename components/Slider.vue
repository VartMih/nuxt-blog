<template>
  <div class="container-fluid home-slider">
    <div id="carouselExampleCaptions" class="carousel slide" data-ride="carousel">
      <ol class="carousel-indicators">
        <li data-target="#carouselExampleCaptions" data-slide-to="0" class="active"></li>
        <li data-target="#carouselExampleCaptions" data-slide-to="1"></li>
        <li data-target="#carouselExampleCaptions" data-slide-to="2"></li>
        <li data-target="#carouselExampleCaptions" data-slide-to="3"></li>
        <li data-target="#carouselExampleCaptions" data-slide-to="4"></li>
      </ol>
      <div class="carousel-inner">
        <div class="carousel-item active">
          <img src="/img/first_img.jpg" class="d-block w-100" alt="Первая картинка">
        </div>
        <div class="carousel-item">
          <img src="/img/second_img.jpg" class="d-block w-100" alt="Вторая картинка">
        </div>
        <div class="carousel-item">
          <img src="/img/third_img.jpg" class="d-block w-100" alt="Третья картинка">
        </div>
        <div class="carousel-item">
          <img src="/img/fourth_img.jpg" class="d-block w-100" alt="Четвертая картинка">
        </div>
        <div class="carousel-item">
          <img src="/img/fifth_img.jpg" class="d-block w-100" alt="Пятая картинка">
        </div>
      </div>
      <a class="carousel-control-prev" href="#carouselExampleCaptions" role="button" data-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="sr-only">Previous</span>
      </a>
      <a class="carousel-control-next" href="#carouselExampleCaptions" role="button" data-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="sr-only">Next</span>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Slider"
}
</script>

<style scoped>

</style>