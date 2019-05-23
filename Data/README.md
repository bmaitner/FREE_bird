#Data files
This folder contains data related to bird traits, including both functional traits and traits related to migration.


###Source: Paul DuFour  
SUPMAT_DUFOUR.docx: Text describing cleaning done on migration data and maps.  
MIGRATION_BIRD_GLOBAL_PD.csv: Data on bird migration characteristics and other functional attributes.  


###Source: Nicolas Loiseau

BirdFuncDat.csv:  the raw database.  
BirdsID.Rdata: ?  
birdstrait.Rdata: the database we are using after cleaning (all functional traits, not in antarctica, phylogenetic information)

The script to select the right trait:

      diet <- prep.fuzzy(birdstrait[,1:10], col.blocks = ncol(birdstrait[,1:10]), label = "diet")
      
      ForStrat <- prep.fuzzy(birdstrait[,11:17], col.blocks = ncol(birdstrait[,11:17]), label = "ForStrat")
      
      bodymass <- log(birdstrait$BodyMass.Value) %>% as.data.frame()
      row.names(bodymass) <- row.names(birdstrait)
      bodymass <- bodymass/(max(bodymass, na.rm=T)-min(bodymass, na.rm=T))
      
      PelagicSpecialist <- as.data.frame(birdstrait$PelagicSpecialist %>% as.character() %>% as.numeric())
      row.names(PelagicSpecialist) <- row.names(birdstrait)
      
      Nocturnal <- as.data.frame(birdstrait$Nocturnal %>% as.character() %>% as.numeric())
      row.names(Nocturnal) <- row.names(birdstrait)

      disTraits_birds <- dist.ktab(ktab.list.df(list(diet, ForStrat,bodymass, PelagicSpecialist, Nocturnal)),
      c("F","F","Q", "D","D"), scan = FALSE) %>% as.matrix()