Futilibird
==========

    require "./setup"
    Dust = require "dust"

    {width, height} = require "./pixie"

    engine = Dust.init
      width: width
      height: height

    global.engine = engine

    bird = engine.add
      x: width/4
      y: height/2
      radius: 30
      color: "blue"
      acceleration: Point(0, 360)

    bird.on "update", ->
      if mousePressed.left or justPressed.any
        bird.I.y -= 20
        bird.I.velocity = Point(0, 0)

    bird.clamp
      y:
        min: 0
        max: height

    [1..3].forEach (n) ->
      pipe = engine.add
        x: n * width/2
        y: height/2
        width: 72
        height: height
        color: "red"
        zIndex: -1

      pipe.on "update", ->
        pipe.I.x -= 1

        if pipe.I.x < -36
          pipe.I.x += 3 * width/2

      pipe.on "draw", (canvas) ->
        # Draw gap
        canvas.drawRect
          color: "black"
          width: 72
          height: 120
          x: -36
          y: bird.I.y - 60 - height/2

    engine.on "beforeDraw", (canvas) ->
      canvas.fill "black" # TODO: Gradient
