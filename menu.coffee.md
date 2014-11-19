Menu
====

    require "tactics-core"
    TWEEN = require "./lib/tween"

    OFF_CAMERA_HEIGHT = 100
    ANIMATION_DURATION = 500

    MENU_ACTIONS = 
      move:
        position:
          x: -15
          y: 20
          z: 0
      attack:
        position:
          x: 15
          y: 20
          z: 0
      magic:
        position:
          x: 0
          y: 20
          z: -15
      wait:
        position:
          x: 0
          y: 20
          z: 15

    geometry = new THREE.BoxGeometry(5, 5, 5)

    THREE.ImageUtils.crossOrigin = ""
    material = new THREE.MeshLambertMaterial
      map: THREE.ImageUtils.loadTexture "https://s3.amazonaws.com/distri-tactics/crate.jpg"

    menuCubes = []
    tweens = []

    module.exports =
      createMenu: (scene) ->
        keyValues MENU_ACTIONS (name, {position}) ->
          {x, y, z} = position
          cube = new THREE.Mesh geometry, material
          cube.position.set x, y + OFF_CAMERA_HEIGHT, z
          cube.userData.action = name

          menuCubes.push cube
          scene.add cube

      openMenu: (position) ->
        {x, y, z} = position

        tweens.length = 0

        menuCubes.forEach (cube) ->
          position = MENU_ACTIONS[cube.userData.action].position
          
          start =
            x: x + position.x
            y: OFF_CAMERA_HEIGHT + (i * 20)
            z: z + position.z

          end =
            x: start.x
            y: y + position.y
            z: start.z

          tween = new TWEEN.Tween(start).to(end, ANIMATION_DURATION)
          tween.easing(TWEEN.Easing.Back.InOut)
          tweens.push
            tween: tween
            cube: cube
          tween.start()

      closeMenu: ->
        tweens.length = 0

        menuCubes.forEach (cube, i) ->
          {x, y, z} = cube.position

          start =
            x: x
            y: y
            z: z

          end =
            x: x
            y: OFF_CAMERA_HEIGHT + (i * 20)
            z: z

          tween = new TWEEN.Tween(start).to(end, ANIMATION_DURATION)
          tween.easing(TWEEN.Easing.Back.InOut)
          tweens.push
            tween: tween
            cube: cube

          tween.start()

      updateMenu: ->
        TWEEN.update()
        tweens.forEach (obj) ->
          obj.tween.onUpdate ->
            obj.cube.position.set @x, @y, @z
