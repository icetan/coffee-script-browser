fs      = require 'fs'
{spawn} = require 'child_process'

runCake = (args, cb) ->
  proc =         spawn 'node', ['bin/cake'].concat(args), cwd:'coffee-script'
  proc.stderr.on 'data', (buffer) -> console.log buffer.toString()
  proc.on        'exit', (status) ->
    process.exit(1) if status != 0
    cb() if typeof cb is 'function'

copyFile = (source, destination, cb) ->
  ins = fs.createReadStream source
  ins.pipe fs.createWriteStream destination
  if typeof cb is 'function'
    ins.on 'end', -> cb()

task 'build', 'build the CoffeeScript language from source', ->
  # UGLY hack setting env option for spawn didn't cut it.
  process.env['MINIFY'] = 'false'
  runCake ['build:browser'], ->
    copyFile 'coffee-script/extras/coffee-script.js', 'index.js', ->
      console.log 'Browser verions built and copied to index.js'
