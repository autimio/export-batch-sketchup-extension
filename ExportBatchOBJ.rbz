=begin
Copyright (c) TIG 2011
Permission to use, copy, modify, and distribute this software for 
any purpose and without fee is hereby granted, provided that the above
copyright notice appear in all copies.
This software is provided "as is" and without any express or
implied warranties, including, without limitation, the implied
warranties of merchantability and fitness for a particular purpose.
Name:
 exportBatch.rb
Usage:
Sketchup Menu > File > Export Batch DAE...
Answer the options dialog, then OK.
[Note that a 'usual' set of 'options' shown - adjust as desired]
Select one SKP from a folder of SKPs to be exported, then OK.
All SKPs in that folder are opened, exported as DAE [using the given 
settings] and then closed.  You might be prompted to save a SKP - answer 
'No'.  If the original SKP was unsaved the last exported SKP remains 
open, otherwise the original SKP's window is restored.
All exported DAE files are found in a [new] subfolder in the selected 
folder called 'DAEs' - there might also be subfolders made to contain 
associated texture files, each named after its DAE.
Any preexisting DAE files will be overwritten without a warning.
Donations:
 PayPal.com info @ revitrev.org
Version:
1.0 20110809 First issue.
1.1 20110812 Inputbox code improved.
=end

#-----------------------------------------------------------------------------
require 'sketchup.rb'
#-----------------------------------------------------------------------------
module TIG
###
def self.exportBatch()
  ###
  model=Sketchup.active_model
  opts= {:doublesided_faces    => true,
         :edges                => false,
         :triangulated_faces   => true,
         :hidden_geometry      => false,
         :preserve_instancing  => true,
         :texture_maps         => true,
         :materials_by_layer   => false,
         :author_attribution   => false,
         :selectionset_only    => false,### always
         :double_precision     => true, ### always
         :vertex_normals       => true  ### always
        }

  ### get user settings
  refs= [:doublesided_faces,
         :edges,
         :triangulated_faces,
         :hidden_geometry,
         :preserve_instancing,
         :texture_maps,
         :materials_by_layer,
         :author_attribution
        ]
  prms= ["Two-Sided Faces: ",
         "Edges: ",
         "Triangulate All Faces: ",
         "Hidden Geometry: ",
         "Preserve Component Hierarchies: ",
         "Texture Maps: ",
         "Color By Layer: ",
         "Preserve Credits: "
        ]
  vals= ["True",
         "False",
         "True",
         "False",
         "True",
         "True",
         "False",
         "False"
        ]
  pops= ["True|False",
         "True|False",
         "True|False",
         "True|False",
         "True|False",
         "True|False",
         "True|False",
         "True|False"
        ]

  titl= "Export TESTE!"
  results=inputbox(prms,vals,pops,titl)
  return nil if not results

  results.each_with_index{|r,i|
    if r=="True"
      opts[refs[i]]=true
    else
      opts[refs[i]]=false
    end
  }
  
  opath=model.path
  if not opath or opath==''
    pwd=Dir.getwd()
  else
    pwd=File.dirname(opath)
  end

  pwd.tr!("\\","/")
  
  if Sketchup.version.to_i <= 13
	ffolder=UI.openpanel("Export Batch STL: Pick Any SKP File + OK...", pwd , "*.skp")
  else
	ffolder=UI.openpanel("Export Batch STL: Pick Any SKP File + OK...", pwd , "SKP|*.skp|")
  end
  
  return nil unless ffolder
  
  folder=File.dirname(ffolder)
  
  return nil if not folder or not File.exist?(folder)

  skps=[]

  Dir.entries(folder).each{|f|skps << File.join(folder,f) if File.extname(f).downcase=='.skp'}

  daes=File.join(folder,'convertion')

  Dir.mkdir(daes) if not File.exist?(daes)

  skps.each{|skp|
    Sketchup.open_file(skp)
    name=File.basename(skp, '.*')
    path=File.join(daes, name+'.obj')
    model.export(path, opts)
  }

  if not opath or opath==''
    Sketchup.send_action('terminate:')
  else
    Sketchup.open_file(opath)
  end
  
end

if not file_loaded?(File.basename(__FILE__))
  UI.menu("File").add_item("Export Batch..."){TIG.exportBatch()}
end

file_loaded(File.basename(__FILE__))

end
