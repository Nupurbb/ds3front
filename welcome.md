---
layout: default
title: Welcome
permalink: /welcome
---

# Welcome ALL TO OUR GREAT WEBSITE                                                   

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Parallax Scrolling</title>
    <style>
        /* Add some basic styling */
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
        }
        .parallax-container {
            height: 200%; /* Set the height to 200% to allow for scrolling */
            width: 100%;
            overflow-x: hidden;
            overflow-y: auto;
            position: relative;
            margin-bottom: 10px; /* Leave 10px of blank space at the bottom */
        }
        .parallax-image {
            background-size: cover;
            background-position: center;
            height: calc(70vh - 3px); /* Set the height to 50% of the viewport height minus 3 pixels */
            width: 100%;
            border: 8px ;
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            align-items: center;
            margin-top: calc(10vh + 3px); /* Center the image vertically */
            position: relative;
        }
        /* Create the parallax scrolling effect */
        .parallax-image {
            background-attachment: fixed;
        } 
        .parallax-image a {
            font-size: 48px; /* Make the font two times bigger */
            font-weight: bold; /* Make the font bold */
            position: absolute;
            top: 10px; /* Adjust vertical position */
            color: white; /* Set text color */
            text-decoration: none; /* Remove underline */
            z-index: 1; /* Ensure the title is above the image */
        }
        /* Customize title colors */
        .parallax-image:nth-child(1) a { color: #6F8DD9; } /* blue */
        .parallax-image:nth-child(2) a { color: #D96FC3; } /* pink */
        .parallax-image:nth-child(3) a { color: #AA6FD9; } /* purple */
        .parallax-image:nth-child(4) a { color: #000000; } /* black */
        /* Add a new color for sleep */
        .parallax-image:nth-child(5) a { color: #897FFF; } /* white */
    </style>
</head>
<body>

<div class="parallax-container">
    <!-- Sets up the links to different pages in the site -->
    <div class="parallax-image" style="background-image: url('https://w0.peakpx.com/wallpaper/595/522/HD-wallpaper-grapefruit-lemonade-pink-drink-lemon-lime.jpg')">
        <a href="http://127.0.0.1:4200/student/drink">Drinks</a>
    </div>
    <div class="parallax-image" style="background-image: url('https://wallpapers.com/images/hd/heartbeat-neon-dark-pink-sdmjcfbn14ol4nvy.jpg')">
        <a href="http://127.0.0.1:4200/student/pulse">Heart Rate</a>
    </div>
    <div class="parallax-image" style="background-image: url('https://img.freepik.com/premium-photo/pink-stationery-pink-background_573856-1067.jpg')">
        <a href="http://127.0.0.1:4200/student/planner">Planner</a>
    </div>
    <div class="parallax-image" style="background-image: url('https://images.squarespace-cdn.com/content/v1/5b147413a9e02837b93f4066/1587961394040-XMRUTY3OL4NT6B50TIXD/PopTennis-14x11.jpg')">
        <a href="http://127.0.0.1:4200/student/Fitness">Fitness</a>
    </div>
    <!-- New section for sleep -->
    <div class="parallax-image" style="background-image: url('https://www.sleep.org.au/image_cache/sleep/default_main_image_001-380x214.jpeg')">
        <a href="http://127.0.0.1:4200/student/sleep">Sleep</a>
    </div>
    <div class="parallax-bar bar1"></div>
    <div class="parallax-bar bar2"></div>
    <div class="parallax-bar bar3"></div>
    <div class="parallax-bar bar4"></div>
</div>

<script>
    /* Sets up the different images used when rotating through images, can add new ones into each list, are all organized based on the different heading they appear under */
    const images = [
        [
            "https://w0.peakpx.com/wallpaper/595/522/HD-wallpaper-grapefruit-lemonade-pink-drink-lemon-lime.jpg",
            "https://t3.ftcdn.net/jpg/02/98/48/06/360_F_298480639_9Xz6EzHwA3MoKPVvM7S6IRzRFbqpBe8A.jpg",
            "https://www.southernliving.com/thmb/RxQPgSjPvE_qqjjNPCgJjJeaats=/1500x0/filters:no_upscale():max_bytes(150000):strip_icc()/4ab70ff2-4373-4805-985f-005ee5256743_0-76feabfb1458442d8aaef8e5bd1bcc7e.png",
            "https://cache.desktopnexus.com/thumbseg/2700/2700303-bigthumbnail.jpg",
            "https://t4.ftcdn.net/jpg/06/57/04/13/360_F_657041372_AUVKfXPALQc9u1L2IXJCRtNQG5MJWOR5.jpg",
            "https://t3.ftcdn.net/jpg/05/09/71/00/360_F_509710059_CdNe0lESIhajbQ5g9MBoJVrzmaNnAbUi.jpg",
            "https://media.istockphoto.com/id/949446594/photo/pomegranate-tequila-cocktail-summer-light-alcoholic-drink-cooling-aperitif-on-light-background.webp?b=1&s=170667a&w=0&k=20&c=PcFX8YOfHeVYT5wfIA5DHQx9XVlrzkvIsL94HKQMTpM="

        ],
        [
            "https://wallpapers.com/images/hd/heartbeat-neon-dark-pink-sdmjcfbn14ol4nvy.jpg",
            "https://www.shutterstock.com/shutterstock/videos/1098556407/thumb/1.jpg?ip=x480",
            "https://img.freepik.com/premium-vector/neon-pink-heart-heart-rate-waves_652844-37.jpg",
            "https://i.vimeocdn.com/video/1374354078-2d24d050ed94af0561937f4870d456e47c8505a91f7971c904488ee21cc346f8-d_640x360.jpg"
        ],
        [
            "https://img.freepik.com/premium-photo/pink-stationery-pink-background_573856-1067.jpg",
            "https://i.pinimg.com/736x/95/d7/26/95d7264d4ac2564650941330b2b43103.jpg",
            "https://as2.ftcdn.net/v2/jpg/01/73/81/93/1000_F_173819337_Z4fv6wOPFCP9UXT1iON1Ev2i4HvzXmsx.jpg",
            "https://marketplace.canva.com/EAFIpxoov1U/1/0/1600w/canva-pink-modern-organizer-desktop-wallpaper-s7kqOFE3dyA.jpg"
        ],
        [
            "https://images.squarespace-cdn.com/content/v1/5b147413a9e02837b93f4066/1587961394040-XMRUTY3OL4NT6B50TIXD/PopTennis-14x11.jpg",
            "https://img.freepik.com/premium-photo/pink-gym-with-sign-that-says-gym-wall_900101-42801.jpg",
            "https://storage.googleapis.com/pai-images/525f12f9c7e14127bc3d3ec94ad07a71.jpeg",
            "https://t4.ftcdn.net/jpg/05/51/40/05/360_F_551400528_cM1cQvQ9ndhizkGGgMjYwAbnrRffa4BR.jpg",
            "https://media.istockphoto.com/id/1129703251/photo/female-sport-equipment-on-a-pink-background.jpg?s=612x612&w=0&k=20&c=uyAnljmel9R01fyFRrb_ThYuWlju77yfVI6UezKyIWU=",
            "https://media.istockphoto.com/id/1310978786/photo/simulator-with-barbell-background-of-pink-wall.jpg?s=612x612&w=0&k=20&c=8JHO6-FFA7-LdOxmqgW4FRbwd2acTs9I_8gfgVYH_AQ="
        ],
        [
            "https://t3.ftcdn.net/jpg/06/59/22/66/360_F_659226684_4V8n8Cv3aNO8ywYKySp8HxW1XNu4ZdCz.jpg",
            "https://img.freepik.com/premium-photo/female-sleep-routine-accessories-pink_185193-43805.jpg",
            "https://media.istockphoto.com/id/1313300521/video/animated-paper-backgrounds-fairy-tale-sleeping-background-4k-cartoon-nature-stock-video.jpg?s=640x640&k=20&c=mI02091qzQ4mhzvR2h8LwZiG8eLQ-PBn3O6P-XGLI4k=",
            "https://img.freepik.com/premium-photo/bed-sleep-stands-pink-clouds-good-soft-sleep_124715-2921.jpg",
            "https://t3.ftcdn.net/jpg/06/40/12/92/360_F_640129265_Pd4QXuL6eDq67svNRiDKryGDND6J42vo.jpg",
            "https://img.freepik.com/premium-photo/pink-bedroom-with-delicate-decor-room-with-furniture-bookshelves-bed-armchair-table-3d-illustration_76964-4339.jpg"
        ]
    ];

    const parallaxImages = document.querySelectorAll('.parallax-image');
    let currentImageIndex = [0, 0, 0, 0, 0];
    // This initializes an array "currentImageIndex" with five elements, all set to 0. This array will keep track of the index of the current image being displayed for each parallax image.

    // Function to change images
    function changeImages() {
        parallaxImages.forEach((image, index) => {
            const imagesIndex = currentImageIndex[index];
            image.style.backgroundImage = `url('${images[index][imagesIndex]}')`;
            currentImageIndex[index] = (imagesIndex + 1) % images[index].length;
        });
    }

    // Initial call to change images
    changeImages();

    // Change images every 3 seconds
    setInterval(changeImages, 3000);
</script>

</body>
</html>
