
# Mongoose cheatsheet
Ce cheatsheet de Mongoose a été réalisé grâce à ⭐ __chatGPT__ ⭐

## Installer Mongoose:

```
npm install mongoose
yarn add mongoose
```

## Utiliser Mongoose:
```
const mongoose = require('mongoose')
```

--------------

## Les bases de mongoose

### Connexion à une base de données MongoDB:

```
const connectionString = "monLienDeConnexionMongoDB"
mongoose.connect(connectionString, options)
```

### Schéma et model

Pour définir un schéma pour vos données et un modèle à partir de ce schéma:
```
const monSchema = mongoose.Schema({
    maPremièreClé: Number,
    maDeuxièmeClé: String 
})
const MonModel = mongoose.model('maCollection', monSchema).
```

### Enregistrer un document dans la base de données:

```
const data = {
    maPremièreClé: 5,
    maDeuxièmeClé: 'hello'
}
const newModel = new Model(data).save().then(data => console.log("modèle sauvegardé"))
```  

### Sous-documents: 

Les sous-documents sont des __objets imbriqués dans un document parent__, ils peuvent être définis en utilisant un __schéma imbriqué__ (exemple : un champ "comments" qui est un tableau de sous-documents dans un document "post").
```
const commentSchema = mongoose.Scheama({
    user: String,
    content: String,
    creationDate: Date
})
const postSchema = mongoose.Scheama({
    user: String,
    content: String,
    creationDate: Date
    comments: [commentSchema] //les crochets permettent de spécifier que la clé comments contiendra un tableau comment basé sur le commentSchema plus haut
})

```

### Relations entre modèles: 

Mongoose prend en charge les relations entre modèles. 
Ces relations peuvent être définies en utilisant des propriétés de type __ObjectId__ dans les schémas de modèles:
```
const commentSchema = mongoose.Scheama({
    user: { type: mongoose.Schema.Types.ObjectId, ref: 'users' }, // la valeur de la clé ref correspond à la collection lié
    content: String,
    creationDate: Date
})
```

Pour récupérer le model, il faudra utiliser les méthodes de Mongoose telles que .populate() pour remplir les données liées:
```
Comment.find({user: userId}).populate('user').then(data => {
		console.log('USERS =>', data);
});
```

### Méthodes mongoose

__`Model.find(query, callback)`__ : cette méthode permet de trouver __plusieurs documents__ en utilisant une requête. La requête peut être utilisée pour filtrer les documents en fonction de certains critères.

__`Model.findOne(query, callback)`__ : cette méthode permet de trouver __un seul document__ en utilisant une requête. Elle est similaire à la méthode find(), mais ne renvoie qu'un seul document.

__`Model.findById(id, callback)`__ : cette méthode permet de trouver __un document en utilisant son ID__. Elle est équivalente à Model.findOne({ _id: id }, callback).

__`Model.findOneAndUpdate(query, update, options, callback)`__ : cette méthode permet de __mettre à jour un document__ en utilisant une requête et de renvoyer le document mis à jour.

__`Model.findOneAndRemove(query, options, callback)`__ : cette méthode permet de __supprimer un document__ en utilisant une requête et de renvoyer le document supprimé.

__`Model.update(query, update, options, callback)`__ : cette méthode permet de __mettre à jour plusieurs documents__ en utilisant une requête.

__`Model.remove(query, callback)`__ : cette méthode permet de _supprimer plusieurs documents_ en utilisant une requête.

__`Model.count(query, callback)`__ : cette méthode permet de __compter les documents__ qui correspondent à une requête.

__`Model.findByIdAndUpdate(id, update, options, callback)`__ : cette méthode permet de __mettre à jour un document en utilisant son ID__. Elle est équivalente à Model.findOneAndUpdate({ _id: id }, update, options, callback).

__`Model.findByIdAndRemove(id, options, callback)`__ : cette méthode permet de __supprimer un document en utilisant son ID__. Elle est équivalente à Model.findOneAndRemove({ _id: id }, options, callback).
    

--------------

    
## Les queries

La requête (ou "query" en anglais) est un objet utilisé pour __filtrer les documents__ dans une base de données MongoDB en utilisant Mongoose. Il est passé en tant que __premier paramètre__ dans de nombreuses méthodes de requête Mongoose. Il est utilisé pour spécifier les critères de filtrage pour les documents que vous voulez trouver, mettre à jour ou supprimer.

La requête est un __objet__ qui contient des __propriétés__ et des __valeurs__ qui correspondent aux champs dans les documents de la base de données. Par exemple, pour trouver tous les documents où le champ "name" est égal à "John", vous pouriez utiliser une requête comme ceci : { name: "John" }.

La requête peut également être utilisée pour spécifier des __opérateurs de comparaison__ comme $gt (greater than), $lt (less than), $in (in) etc. Pour trouver tous les documents où le champ "age" est supérieur à 30, vous pourriez utiliser une requête comme ceci : { age: { $gt: 30 } }.

### Les principaux opérateurs de comparaison utilisés dans les requêtes Mongoose :

__$eq__ (égal) : cet opérateur permet de trouver les documents où la valeur d'un champ est __égale__ à une valeur spécifiée. 
Exemple : 
```
{ age: { $eq: 30 } } (trouve tous les documents où l'âge est égal à 30)
```

__$ne__ (non égal) : cet opérateur permet de trouver les documents où la valeur d'un champ n'est __pas égale__ à une valeur spécifiée. 
Exemple : 
```
{ age: { $ne: 30 } } (trouve tous les documents où l'âge n'est pas égal à 30)
```

__$gt__ (greater than - supérieur) : cet opérateur permet de trouver les documents où la valeur d'un champ est __supérieure__ à une valeur spécifiée.
Exemple : 
```
{ age: { $gt: 30 } } (trouve tous les documents où l'âge est supérieur à 30)
```

__$gte__ (greater than or equal to - supérieur ou égal) : cet opérateur permet de trouver les documents où la valeur d'un champ est __supérieure ou égale__ à une valeur spécifiée. 
Exemple : 
```
{ age: { $gte: 30 } } (trouve tous les documents où l'âge est supérieur ou égal à 30)
```

__$lt__ (less than - inférieur) : cet opérateur permet de trouver les documents où la valeur d'un champ est __inférieure__ à une valeur spécifiée. 
Exemple : 
```
{ age: { $lt: 30 } } (trouve tous les documents où l'âge est inférieur à 30)
```

__$lte__ (less than or equal to - inférieur ou égal) : cet opérateur permet de trouver les documents où la valeur d'un champ est __inférieure ou égale__ à une valeur spécifiée. 
Exemple : 
```
{ age: { $lte: 30 } } (trouve tous les documents où l'âge est inférieur ou égal à 30)
```

__$in__ (in) : cet opérateur permet de trouver les documents où la __valeur d'un champ est contenue dans un tableau spécifié__. 
Exemple : 
```
{ age: { $in: [25, 30, 35] } } (trouve tous les documents où l'âge est 25, 30 ou 35)
```

__$nin__ (not in) : cet opérateur permet de trouver les documents où la valeur d'un __champ n'est pas contenue dans un tableau spécifié__. 
Exemple : 
```
{ age: { $nin: [25, 30, 35] } } (trouve tous les documents où l'âge n'est pas 25, 30 ou 35)
```

__$exists__ : cet opérateur permet de trouver les documents où un __champ existe ou n'existe pas__. 
Exemple : 
```
{ age: { $exists: true } } (trouve tous les documents où le champ "âge" existe)
```

__$regex__ : cet opérateur permet de trouver les documents où la valeur d'un champ correspond à une __expression régulière__ spécifiée. 
Exemple : 
```
{ name: { $regex: /^J/ } } (trouve tous les documents où le nom commence par "J")
```

__$elemMatch__ : cet opérateur permet de trouver les documents où un __élément d'un tableau correspond à une requête__ spécifiée. 
Exemple : 
```
{ comments: { $elemMatch: { author: "John", votes: { $gte: 5 } } } } // trouve tous les documents où il y a un commentaire écrit par "John" et ayant au moins 5 votes
```

--------------
    
## Les méthodes personnalisées

Les méthodes personnalisées dans Mongoose sont des __fonctions__ qui peuvent être __ajoutées à un modèle__ pour effectuer des opérations spécifiques. Ces méthodes peuvent être utilisées pour mettre à jour les données liées ou pour calculer des statistiques.

Pour ajouter une méthode personnalisée à un modèle, il suffit de définir une fonction sur le prototype du modèle. 

### Exemple 1: mettre à jour les données liées 

Par exemple, pour ajouter une méthode pour mettre à jour les données liées :

    const userSchema = new mongoose.Schema({
        name: { type: String },
        age: { type: Number },
        relatedData: { type: Schema.Types.ObjectId, ref: 'RelatedData' } //la collection users a un lien avec la collection RelatedData
    });

    userSchema.methods.updateRelatedData = function(relatedData) {
        this.relatedData = relatedData;
        return this.save();
    };

    const User = mongoose.model('User', userSchema);

Cette méthode personnalisée prend en entrée un objet relatedData et l'affecte à la propriété relatedData de l'instance. Puis elle sauvegarde l'instance pour que les modifications soient persistées en base de données.

Cette méthode peut être utilisée comme ceci :

    const user = new User({ name: 'John', age: 30 });

    // Créer une nouvelle instance de l'objet RelatedData avec les propriétés spécifiées
    const relatedData = new RelatedData({...}); 

    // Utiliser la méthode updateRelatedData pour mettre à jour les données liées de l'utilisateur avec les données de relatedData
    user.updateRelatedData(relatedData).then(() => console.log('related data updated'));

Il est important de noter que cette méthode suppose que le __modèle RelatedData existe__ et est __correctement configuré__ pour être utilisé avec Mongoose.
Ce code utilise une méthode personnalisée "updateRelatedData" pour mettre à jour les données liées d'un utilisateur avec les données d'un objet "relatedData" créé précédemment. La méthode updateRelatedData utilise la méthode save de mongoose pour sauvegarder les modifications sur la base de données.

### Exemple 2: calculer des statistiques

Pour ajouter une méthode pour calculer des statistiques il suffit de définir une fonction sur le prototype du modèle :

    const articleSchema = new mongoose.Schema({
        title: { type: String },
        views: { type: Number }
    });

    articleSchema.methods.calculateStatistics = function() {
        // code to calculate statistics
        // exemple :
        return {
            averageViews: this.views / this.length
        };
    };

    const Article = mongoose.model('Article', articleSchema);


Ces méthodes personnalisées peuvent ensuite être appelées sur les instances de modèles. 
Par exemple :

    const user = new User({ name: 'John', age: 30 });
    user.updateRelatedData({ relatedData });

    const article = new Article({ title: 'My Article', views: 100 });
    const statistics = article.calculateStatistics();
    console.log(statistics);


Il est important de noter que les méthodes personnalisées sont des fonctions qui s'exécutent dans le __contexte de l'instance de modèle__, c'est pourquoi l'accès aux propriétés de l'instance se fait via `this`. 

--------------

## Les middlewares 

Les middlewares Mongoose sont des fonctions qui sont exécutées __avant ou après certaines opérations sur un modèle__ Mongoose. Il existe plusieurs types de middlewares :

### Middlewares d'enregistrement

__pre('save')__ : ces middlewares sont exécutés __avant__ l'enregistrement d'un document. Ils peuvent être utilisés pour valider les données, ajouter des propriétés calculées, etc. 
Exemple du chiffrement du mot de passe d'un utilisateur avant l'envoi à la base de données:

    schema.pre('save', function(next) {
        if (!this.isModified('password')) {
            return next();
        }
        bcrypt.hash(this.password, 8, (err, hash) => {
            if (err) {
            return next(err);
            }
            this.password = hash;
            next();
        });
    });

__post('save')__ : ces middlewares sont exécutés __après__ l'enregistrement d'un document. Ils peuvent être utilisés pour envoyer des notifications, journaliser des actions, etc.

    schema.post('save', function(doc, next) {
        console.log(`${doc.name} has been saved`);
        next();
    });

Il est important de noter que ces middlewares sont exécutés __uniquement__ lors de l'enregistrement d'un __nouveau document__ ou de la __modification d'un document__ existant via la __méthode save()__. Pour les autres méthodes d'enregistrement comme create() ou insertMany() il faut utiliser les middlewares appropriées. 

### Middlewares de validation

Les middlewares de validation dans Mongoose sont des fonctions qui sont exécutées __avant__ l'enregistrement d'un document. Ils permettent de valider les données d'un document avant qu'il ne soit enregistré. Il existe plusieurs types de middlewares de validation :

__validate__ : ces middlewares sont exécutés __avant__ l'enregistrement d'un document. Ils peuvent être utilisés pour valider les propriétés d'un document.
Exemple d'un validateur d'adresses mail:

    const schema = new mongoose.Schema({
        email: {
            type: String,
            validate: {
            validator: function(value) {
                return /^[a-zA-Z0-9.!#$%&’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/.test(value);
            },
            message: props => `${props.value} is not a valid email address!`
            },
            required: [true, 'User email address is required']
        }
    });

__validateSync__ : ces middlewares sont exécutés __avant__ l'enregistrement d'un document. Ils peuvent être utilisés pour valider les propriétés d'un document. Cette méthode est synchronisée contrairement à validate qui est asynchrone. 
Exemple une application mobile qui interdirait l'inscription des personnes agées de moins de 18 ans:

    const schema = new mongoose.Schema({
        age: {
            type: Number,
            validateSync: function(value) {
                if (value < 18) {
                    throw new Error('Age must be greater than 18');
                }
            }
        }
    });

Il est important de noter que les erreurs de validation sont généralement collectées dans un tableau de __validationError__.

### Middlewares de suppression

Les middlewares de suppression dans Mongoose sont des fonctions qui sont exécutées __avant ou après la suppression d'un document__. Il existe plusieurs types de middlewares de suppression :

__pre('remove')__ : ces middlewares sont exécutés __avant__ la suppression d'un document. Ils peuvent être utilisés pour effectuer des actions avant la suppression d'un document, comme la suppression d'autres documents liés. 
Exemple:

    schema.pre('remove', function(next) {
        this.model('Comment').deleteMany({ post: this._id }, next);
    });

__post('remove')__ : ces middlewares sont exécutés __après__ la suppression d'un document. Ils peuvent être utilisés pour effectuer des actions après la suppression d'un document, comme l'envoi de notifications, la journalisation des actions, etc. 
Exemple :

    schema.post('remove', function(doc) {
        console.log(`${doc.name} has been removed`);
    });

Il est important de noter que ces middlewares sont exécutés __uniquement__ lors de la __suppression d'un document via la méthode remove()__. Pour les autres méthodes de suppression comme deleteOne() ou deleteMany() il faut utiliser les middlewares appropriés.
