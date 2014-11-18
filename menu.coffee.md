Menu
====

    TWEEN = require "./lib/tween"
  
    OFF_CAMERA_HEIGHT = 100
  
    OFFSETS = [
      { x: -15, y: 20, z: 0 }
      { x: 15, y: 20, z: 0 }
      { x: 0, y: 20, z: -15 }
      { x: 0, y: 20, z: 15 }
    ]    
    
    geometry = new THREE.BoxGeometry(5, 5, 5)
    material = new THREE.MeshBasicMaterial
      color: 0xfffff    
    
    # TODO: fix s3 cross domain nonsense
    # material = new THREE.MeshLambertMaterial
    #   map: THREE.ImageUtils.loadTexture "https://s3.amazonaws.com/distri-tactics/crate.jpg"
    
    menuCubes = []
    tweens = []

    module.exports = 
      createMenu: (scene) ->
        OFFSETS.forEach (offset) ->
          cube = new THREE.Mesh geometry, material
          cube.position.set offset.x, offset.y + OFF_CAMERA_HEIGHT, offset.z
          
          menuCubes.push cube
          scene.add cube        
    
      openMenu: (position) ->       
        {x, y, z} = position

        tweens.length = 0

        menuCubes.forEach (cube, i) ->
          start = 
            x: x + OFFSETS[i].x
            y: OFF_CAMERA_HEIGHT
            z: z + OFFSETS[i].z
            
          end = 
            x: start.x 
            y: y + OFFSETS[i].y
            z: start.z
            
          tween = new TWEEN.Tween(start).to(end, 1000)
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
            y: OFF_CAMERA_HEIGHT
            z: z
            
          tween = new TWEEN.Tween(start).to(end, 1000)
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
