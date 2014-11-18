Menu
====

    require "./lib/tween"
  
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

    module.exports = 
      createMenu: (scene) ->
        OFFSETS.forEach (offset) ->
          cube = new THREE.Mesh geometry, material
          cube.position.set offset.x, offset.y + 500, offset.z
          
          menuCubes.push cube
          scene.add cube        
    
      openMenu: (position) ->       
        {x, y, z} = position

        menuCubes.forEach (cube, i) ->
          cube.visible = true
          cube.position.set x + OFFSETS[i].x, y + OFFSETS[i].y, z + OFFSETS[i].z
      
      closeMenu: ->
        menuCubes.forEach (cube) ->
          cube.position.setY 500
