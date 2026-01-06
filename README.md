# mousefollower
(function () {
  var img = document.createElement("img");

  var walkSrc = "WALK_URL";
  var idleSources = [
    "IDLE1_URL",
    "IDLE2_URL",
    "IDLE3_URL"
  ];

  img.src = idleSources[0];
  img.style.position = "fixed";
  img.style.width = "60px";
  img.style.pointerEvents = "none";
  img.style.zIndex = "9999";
  img.style.left = "0px";
  img.style.top = "0px";

  document.body.appendChild(img);

  var targetX = 0;
  var targetY = 0;
  var currentX = 0;
  var currentY = 0;
  var moving = false;
  var stopTimer;
  var lastIdle = null;

  document.addEventListener("mousemove", function (e) {
    targetX = e.clientX - img.offsetWidth / 2;
    targetY = e.clientY - img.offsetHeight / 2;

    if (!moving) {
      img.src = walkSrc;
      moving = true;
    }

    clearTimeout(stopTimer);
    stopTimer = setTimeout(function () {
      var next;
      do {
        next = idleSources[Math.floor(Math.random() * idleSources.length)];
      } while (next === lastIdle && idleSources.length > 1);

      img.src = next;
      lastIdle = next;
      moving = false;
    }, 400);
  });

  function animate() {
    currentX += (targetX - currentX) * 0.15;
    currentY += (targetY - currentY) * 0.15;

    img.style.left = currentX + "px";
    img.style.top = currentY + "px";

    requestAnimationFrame(animate);
  }

  animate();
})();
