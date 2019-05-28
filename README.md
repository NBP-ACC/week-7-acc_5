# week-7-acc_5
week-7-acc_5 created by GitHub Classroom


#displayTrial = df[df["trial"]==randomTrial]
randomTrial = float(df.sample()["trial"])
trialDF = df[df["trial"] == randomTrial]

fig, axes = plt.subplots(nrows = 2, ncols = 2, figsize=(15,10))
axes = axes.flatten()
#cmap = plt.cm.get_cmap('Spectral')


#grouping Dataframe by category to access categories individually
grouped = trialDF.groupby("category")

#iterating through the four different categories
for i, category in enumerate([7.0, 8.0, 10.0, 11.0]):
    currentCat = grouped.get_group(category)
    #randomly choose a subject that performed the selected trial in the current category
    randomSUB = random.choice(currentCat.SUBJECTINDEX.unique())
    #this is our final trial performed by the random subject
    selectedTrial = currentCat[currentCat["SUBJECTINDEX"] == randomSUB]
    #read out the corresponding filenumber indicating the image used for the subject in that trial
    filenumber = float(selectedTrial.filenumber.unique())
    #load image
    image = openRandomPNG(category, filenumber)
    #some layout settings
    axes[i].set_title(categories[category], fontsize=15, pad=15)
    axes[i].axis("off")
    #plot saccadepath
     #helper to be able to achieve different colour of circles 
    selectedTrial["normalizedTime"] = pd.to_numeric(normalize(selectedTrial["start"]))
   
    cmap = plt.cm.get_cmap('Reds')    
    #for the dots
    axes[i].scatter(x = selectedTrial["x"], 
                    y = selectedTrial["y"], 
                    alpha = 0.7,
                    s = selectedTrial["duration"] /5, 
                    c=cmap(selectedTrial["normalizedTime"]))
    
    #for the lines
    axes[i].plot(selectedTrial["x"], 
                 selectedTrial["y"], 
                 alpha = 0.7,
                 c="white")
    
    axes[i].imshow(image)
    selectedTrial = pd.DataFrame()
