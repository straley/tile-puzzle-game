<script lang="ts">
  export let width="100px"
  export let height="100px"
  export let backgroundColor="#ffffff"

  export let palette = 0
  export let quantity = 3
  export let layers = [0,0,0,0]

  type Shapes = {[index:string]: string}

  type SVGShape = {
    type?: string
    d?: string
    points?: string
    transform?: string
    cx?: number
    cy?: number
    r?: number
    x?: number
    y?: number
    width?: number
    height?: number
    opacity?: number
    backgroundColor?: string
  }

  type Composition = (quantity?: number) => SVGShape[]
  type Compositions = {[index: string]: Composition}

  type Layer = {
    composition: string
    fill: string
    quantity?: number  
  }

  function spin(template:object, quantity:number) {
    return new Array(quantity).fill(0).map((_, i) => ({...template, transform: `rotate(${(360 / quantity) * i}, 50, 50)`}))
  }

  const shapes: Shapes = {
    chevron: "25,10 40,10 50,25 60,10 75,10 50,50",
    chevronUp: "25,40 40,40 50,25 60,40 75,40 50,0",
    drop: "M50 3 Q51.5 6.8 60 18 A12.8 12.8 0 1 1 40 18z",
    dropUp: "M50 37 Q51.5 33.2 60 22 A12.8 12.8 0 1 0 40 22z",
    petal: "M 45 35 C 45 32.24 47.24 20 50 20 C 52.76 20 55 32.24 55 35 C 55 37.76 52.76 50 50 50 C 47.24 50 45 37.76 45 35 Z",
    baton: "M 52.5 44.509 C 51.764 47.123 48.236 47.123 47.5 44.509 L 47.5 32 Q 45.797 32.334 45.485 30 C 45.174 27.666 45 25.77 45 25 C 45 22.24 47.24 20 50 20 C 52.76 20 55 22.24 55 25 C 55 25.77 54.826 27.666 54.515 30 Q 54.203 32.334 52.5 32 L 52.5 44.509 Z",
    banner: "M 46 38 L 44 32 L 46 26 L 44 20 L 46 22 L 48 20 L 50 22 L 52 20 L 54 22 L 56 20 L 54 26 L 56 32 L 54 38 L 56 44 L 44 44 L 46 38 Z",
    fork: "M 46 38 L 42 30 L 42 20 L 46 20 L 46 28 L 48 28 L 48 20 L 52 20 L 52 28 L 54 28 L 54 20 L 58 20 L 58 30 L 54 38 L 54 44 L 46 44 L 46 38 Z",
    pinwheel: "M 52 30 L 50 18 L 50 18 L 62 28 L 52 44 L 50 44 L 50 44 L 52 30 Z",
    snowman: "M 44.462 31.261 C 44.158 30.57 44 29.803 44 29 C 44 26.957 45.023 25.152 46.595 24.082 C 46.215 23.48 46 22.765 46 22 C 46 19.792 47.792 18 50 18 C 52.208 18 54 19.792 54 22 C 54 22.765 53.785 23.48 53.405 24.082 C 54.977 25.152 56 26.957 56 29 C 56 29.803 55.842 30.57 55.538 31.261 C 57.062 32.7 58 34.741 58 37 C 58 41.415 54.415 45 50 45 C 45.585 45 42 41.415 42 37 C 42 34.741 42.938 32.7 44.462 31.261 Z",
    hugeSquare: "25,10 75,10 75,70 25,70",
    largeSquare: "30,15 70,15 70,55 30,55",
    mediumSquare: "35,20 65,20 65,50 35,50",
    smallSquare: "40,25 60,25 60,45 40,45",
    tinySquare: "45,30 55,30 55,40 45,40",
    hugeCircle: "M 25 25 C 25 11.202 36.202 0 50 0 C 63.798 0 75 11.202 75 25 C 75 38.798 63.798 50 50 50 C 36.202 50 25 38.798 25 25 Z",
    largeCircle: "M 30 25 C 30 13.962 38.962 5 50 5 C 61.038 5 70 13.962 70 25 C 70 36.038 61.038 45 50 45 C 38.962 45 30 36.038 30 25 Z",
    mediumCircle: "M 35 25 C 35 16.721 41.721 10 50 10 C 58.279 10 65 16.721 65 25 C 65 33.279 58.279 40 50 40 C 41.721 40 35 33.279 35 25 Z",
    smallCircle: "M 40 25 C 40 19.481 44.481 15 50 15 C 55.519 15 60 19.481 60 25 C 60 30.519 55.519 35 50 35 C 44.481 35 40 30.519 40 25 Z",
    tinyCircle: "M 45 25 C 45 22.24 47.24 20 50 20 C 52.76 20 55 22.24 55 25 C 55 27.76 52.76 30 50 30 C 47.24 30 45 27.76 45 25 Z"
  }

  const compositions: Compositions = {
    fullSquareCenter: () => [{type: "rect", x:0, y:0, width:100, height:100}],
    fullCheckerBoard: () => [
      {type: "rect", x:0, y:0, width:25, height: 25},
      {type: "rect", x:25, y:0, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:50, y:0, width:25, height: 25},
      {type: "rect", x:75, y:0, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:0, y:25, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:25, y:25, width:25, height: 25},
      {type: "rect", x:50, y:25, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:75, y:25, width:25, height: 25},
      {type: "rect", x:0, y:50, width:25, height: 25},
      {type: "rect", x:25, y:50, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:50, y:50, width:25, height: 25},
      {type: "rect", x:75, y:50, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:0, y:75, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:25, y:75, width:25, height: 25},
      {type: "rect", x:50, y:75, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:75, y:75, width:25, height: 25},
    ],
    fullCheckerBoardUp: () => [
      {type: "rect", x:0, y:0, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:25, y:0, width:25, height: 25},
      {type: "rect", x:50, y:0, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:75, y:0, width:25, height: 25},
      {type: "rect", x:0, y:25, width:25, height: 25},
      {type: "rect", x:25, y:25, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:50, y:25, width:25, height: 25},
      {type: "rect", x:75, y:25, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:0, y:50, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:25, y:50, width:25, height: 25},
      {type: "rect", x:50, y:50, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:75, y:50, width:25, height: 25},
      {type: "rect", x:0, y:75, width:25, height: 25},
      {type: "rect", x:25, y:75, width:25, height: 25, opacity: 0.75},
      {type: "rect", x:50, y:75, width:25, height: 25},
      {type: "rect", x:75, y:75, width:25, height: 25, opacity: 0.75},
    ],
    fullBullseye: () => [
      {type: "circle", cx: 50, cy:50, r:130, backgroundColor},
      {type: "circle", cx: 50, cy:50, r:130, opacity: 0.75},
      {type: "circle", cx: 50, cy:50, r:110},
      {type: "circle", cx: 50, cy:50, r:90, backgroundColor},
      {type: "circle", cx: 50, cy:50, r:90, opacity: 0.75},
      {type: "circle", cx: 50, cy:50, r:70},
      {type: "circle", cx: 50, cy:50, r:50, backgroundColor},
      {type: "circle", cx: 50, cy:50, r:50, opacity: 0.75},
      {type: "circle", cx: 50, cy:50, r:30},
      {type: "circle", cx: 50, cy:50, r:10, backgroundColor},
      {type: "circle", cx: 50, cy:50, r:10, opacity: 0.75},
    ],
    fullTriangles: () => [
      {type: "polygon", points: "0,0 0,25 25,25", opacity: 0.75},
      {type: "polygon", points: "0,0 25,25 50,0"},
      {type: "polygon", points: "25,25 50,0 75,25", opacity: 0.75},
      {type: "polygon", points: "50,0 75,25 100,0"},
      {type: "polygon", points: "75,25 100,0 100,25", opacity: 0.75},
      {type: "polygon", points: "0,25 25,25 0,50"},
      {type: "polygon", points: "0,50 25,25 50,50", opacity: 0.75},
      {type: "polygon", points: "25,25 50,50 75,25"},
      {type: "polygon", points: "50,50 75,25 100,50", opacity: 0.75},
      {type: "polygon", points: "75,25 100,25 100,50"},

      {type: "polygon", points: "0,50 0,75 25,75", opacity: 0.75},
      {type: "polygon", points: "0,50 25,75 50,50"},
      {type: "polygon", points: "25,75 50,50 75,75", opacity: 0.75},
      {type: "polygon", points: "50,50 75,75 100,50"},
      {type: "polygon", points: "75,75 100,50 100,75", opacity: 0.75},
      
      {type: "polygon", points: "0,75 25,75 0,100"},
      {type: "polygon", points: "0,100 25,75 50,100", opacity: 0.75},
      {type: "polygon", points: "25,75 50,100 75,75"},
      {type: "polygon", points: "50,100 75,75 100,100", opacity: 0.75},
      {type: "polygon", points: "75,75 100,75 100,100"},
    ],
    fullTrianglesUp: () => [
      {type: "polygon", points: "0,100 0,75 25,75", opacity: 0.75},
      {type: "polygon", points: "0,100 25,75 50,100"},
      {type: "polygon", points: "25,75 50,100 75,75", opacity: 0.75},
      {type: "polygon", points: "50,100 75,75 100,100"},
      {type: "polygon", points: "75,75 100,100 100,75", opacity: 0.75},

      {type: "polygon", points: "0,75 25,75 0,50"},
      {type: "polygon", points: "0,50 25,75 50,50", opacity: 0.75},
      {type: "polygon", points: "25,75 50,50 75,75"},
      {type: "polygon", points: "50,50 75,75 100,50", opacity: 0.75},
      {type: "polygon", points: "75,75 100,75 100,50"},

      {type: "polygon", points: "0,50 0,25 25,25", opacity: 0.75},
      {type: "polygon", points: "0,50 25,25 50,50"},
      {type: "polygon", points: "25,25 50,50 75,25", opacity: 0.75},
      {type: "polygon", points: "50,50 75,25 100,50"},
      {type: "polygon", points: "75,25 100,50 100,25", opacity: 0.75},
      
      {type: "polygon", points: "0,25 25,25 0,0"},
      {type: "polygon", points: "0,0 25,25 50,0", opacity: 0.75},
      {type: "polygon", points: "25,25 50,0 75,25"},
      {type: "polygon", points: "50,0 75,25 100,0", opacity: 0.75},
      {type: "polygon", points: "75,25 100,25 100,0"},
    ],
    largeCircleCenter: () => [{type: "circle", cx: 50, cy:50, r:35}],
    mediumCircleCenter: () => [{type: "circle", cx: 50, cy:50, r:20}],
    smallCircleCenter: () => [{type: "circle", cx: 50, cy:50, r:7.5}],
    chevron: (quantity:number) => spin({type: "polygon", points: shapes["chevron"]}, quantity),
    chevronUp: (quantity:number) => spin({type: "polygon", points: shapes["chevronUp"]}, quantity),
    drop: (quantity:number) => spin({type: "path", d: shapes["drop"]}, quantity),
    dropUp: (quantity:number) => spin({type: "path", d: shapes["dropUp"]}, quantity),
    petal: (quantity:number) => spin({type: "path", d: shapes["petal"]}, quantity),
    baton: (quantity:number) => spin({type: "path", d: shapes["baton"]}, quantity),
    banner: (quantity:number) => spin({type: "path", d: shapes["banner"]}, quantity),
    fork: (quantity:number) => spin({type: "path", d: shapes["fork"]}, quantity),
    pinwheel: (quantity:number) => spin({type: "path", d: shapes["pinwheel"]}, quantity),
    snowman: (quantity:number) => spin({type: "path", d: shapes["snowman"]}, quantity),
    hugeSquare: (quantity:number) => spin({type: "polygon", points: shapes["hugeSquare"]}, quantity),
    largeSquare: (quantity:number) => spin({type: "polygon", points: shapes["largeSquare"]}, quantity),
    mediumSquare: (quantity:number) => spin({type: "polygon", points: shapes["mediumSquare"]}, quantity),
    smallSquare: (quantity:number) => spin({type: "polygon", points: shapes["smallSquare"]}, quantity),
    tinySquare: (quantity:number) => spin({type: "polygon", points: shapes["tinySquare"]}, quantity),
    hugeCircle: (quantity:number) => spin({type: "path", d: shapes["hugeCircle"]}, quantity),
    largeCircle: (quantity:number) => spin({type: "path", d: shapes["largeCircle"]}, quantity),
    mediumCircle: (quantity:number) => spin({type: "path", d: shapes["mediumCircle"]}, quantity),
    smallCircle: (quantity:number) => spin({type: "path", d: shapes["smallCircle"]}, quantity),
    tinyCircle: (quantity:number) => spin({type: "path", d: shapes["tinyCircle"]}, quantity),
  }

  const background = [
    "fullSquareCenter", "fullCheckerBoard", "fullCheckerBoardUp", "fullBullseye", "fullTriangles", "fullTrianglesUp",
  ][layers[0]]

  const layer1 = [
    "hugeSquare", "largeSquare", "mediumSquare", "hugeCircle", "largeCircle", "mediumCircle"
  ][layers[1]]

  const layer2 = [
    "chevron", "chevronUp", "drop", "dropUp", "smallSquare", "tinySquare", "smallCircle", "tinyCircle"
  ][layers[2]]

  const layer3 = [
    "mediumCircleCenter", "smallCircleCenter", "petal", "baton", "banner", "fork", "pinwheel", "snowman"
  ][layers[3]]


  // from https://colorhunt.co/
  const colors = [
    ["#ff75a0", "#ff75a0", "#eaffd0", "#95e1d3"],
    ["#c5d7bd", "#9fb8ad", "#383e56", "#fb743e"],
    ["#a1cae2", "#c2b092", "#cfc5a5", "#eae3c8"],
    ["#faf3e0", "#eabf9f", "#b68973", "#1e212d"],
    ["#99bbad", "#ebd8b7", "#c6a9a3", "#9a8194"],
    ["#91091e", "#da723c", "#c39e5c", "#fdf1d6"],
    ["#822659", "#b34180", "#e36bae", "#f8a1d1"],
    ["#f4f9f9", "#ccf2f4", "#a4ebf3", "#aaaaaa"],
    ["#f4f9f9", "#f1d1d0", "#fbaccc", "#f875aa"],
    ["#e7e6e1", "#f7f6e7", "#f2a154", "#314e52"],
    ["#ffee93", "#f5d782", "#e97878", "#9b5151"],
    ["#d8c292", "#5b5b5b", "#b67171", "#c19065"],
  ]

  const fills = colors[palette]

  const display: Layer[] = [
    { composition: background, fill: fills[0], quantity: 1},      
    { composition: layer1, fill: fills[1], quantity },
    { composition: layer2, fill: fills[2], quantity },
    { composition: layer3, fill: fills[3], quantity },
  ]

</script>

<svg {width} {height}>
  {#each display as {composition, fill, quantity}}
    {#each compositions[composition](quantity) as {type, d, points, transform, cx, cy, r, x, y, width, height, opacity, backgroundColor} }
      {#if type === "circle"}
      <circle {cx} {cy} {r} fill={backgroundColor || fill} opacity={typeof opacity === "number" ? opacity : 1}/>
      {/if}
      {#if type === "path"}
        <path {d} {transform} fill={backgroundColor || fill} opacity={typeof opacity === "number" ? opacity : 1}/>
      {/if}
      {#if type === "polygon"}
        <polygon {points} {transform} fill={backgroundColor || fill} opacity={typeof opacity === "number" ? opacity : 1}/>
      {/if}
      {#if type === "rect"}
        <rect {x} {y} {width} {height} fill={backgroundColor || fill} opacity={typeof opacity === "number" ? opacity : 1}/>
      {/if}
    {/each}
  {/each}
</svg>

<style>
  svg {
    margin: 0;
    padding: 0;
    display: inline-block;
  }
</style>