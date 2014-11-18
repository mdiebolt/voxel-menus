Menu
====
    
    module.exports = (position, scene) ->
      geometry = new THREE.BoxGeometry(5, 5, 5)
      material = new THREE.MeshBasicMaterial
        color: 0xfffff
      
      # TODO: fix s3 cross domain nonsense
      # material = new THREE.MeshLambertMaterial
      #   map: THREE.ImageUtils.loadTexture "https://s3.amazonaws.com/distri-tactics/crate.jpg"
        
      {x, y, z} = position
      
      [
        { x: -15, y: 20, z: 0 }
        { x: 15, y: 20, z: 0 }
        { x: 0, y: 20, z: -15 }
        { x: 0, y: 20, z: 15 }
      ].forEach (offset) ->
        cube = new THREE.Mesh geometry, material
        cube.position.set x + offset.x, y + offset.y, z + offset.z
        scene.add cube