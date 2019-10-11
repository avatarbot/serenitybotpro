    const Discord = require('discord.js')
    const client = new Discord.Client()
    const config = require("./config.json")
    const prefix = config.prefix
    const moment = require ("moment")
    const ms = require("ms");
    const sm = require("string-similarity");

    client.login(config.token)

    client.on('ready', () => {
        console.log(`${client.user.tag} est desormais connecté sur Serenity !`)
        client.user.setActivity('Serenity E-Sport - s!help')
    })

    client.on("message", async message => {
        const args = message.content.split(/ +/g).slice(1);
        if (message.content.startsWith(prefix + "help") || message.content.startsWith("h")) {
            let embed = new Discord.RichEmbed()
            .setTitle("Serenity E-Sport")
            .addField("General", "`ping`, `say`, `sondage`")
            .addField("Moderation", "`ban`, `kick`, `mute`, `unmute`")
            .addField("Information", "`serverinfo`, `userinfo`")
            .setTimestamp()
            .setColor('RED')
            message.channel.send(embed)
        }
        if (message.content.startsWith(prefix + "ban")) {
        if (!message.guild.member(message.author).hasPermission('BAN_MEMBERS')) { return message.channel.send('Vous n\'avez pas la permission !'); }
        if (!message.guild.member(client.user).hasPermission('BAN_MEMBERS')) { return message.channel.send('Le bot n\'a pas la permission !'); }
        if (message.mentions.users.size === 0) { return message.channel.send('Vous devez mentionner un utilisateur !'); }
        let banMember = message.guild.member(message.mentions.users.first());
        if (!banMember) { return message.channel.send('Je n\'ai pas trouvé l\'utilisateur !'); }
        message.mentions.users.first().send(`Vous êtes banni du serveur **${message.guild.name}** par ${message.author.username}`)
        .then(() => {
            banMember.ban()
                .then((member) => {
                    message.channel.send(`${member.user.username} est ban ! Par ${message.author.username}`);
                })
                    .catch((err) => {
                        if (err) { return console.error(err); }
                    });
            })
            .catch((error) => {
                if (error) { console.error(error); }
                    banMember.ban()
                        .then((member) => {
                            message.channel.send(`${member.user.username} est ban ! Par ${message.author.username}`);
                        })
                            .catch((err) => {
                                if (err) { return console.error(err); }
                            });
            });
        }
        if (message.content.startsWith(prefix + "kick")) {
            if (!message.guild.member(message.author).hasPermission('KICK_MEMBERS')) { return message.channel.send('Vous n\'avez pas la permission !'); }
            if (!message.guild.member(client.user).hasPermission('KICK_MEMBERS')) { return message.channel.send('Le bot n\'a pas la permission !'); }
            if (message.mentions.users.size === 0) { return message.channel.send('Vous devez mentionner un utilisateur !'); }
        
                let kickMember = message.guild.member(message.mentions.users.first());
                if (!kickMember) { return message.channel.send('Je n\'ai pas trouvé l\'utilisateur !'); }
            
                message.mentions.users.first().send(`Vous êtes kick du serveur **${message.guild.name}** par ${message.author.username}`)
                    .then(() => {
                        kickMember.kick()
                            .then((member) => {
                                message.channel.send(`${member.user.username} est kick ! Par ${message.author.username}`);
                            })
                                .catch((err) => {
                                    if (err) { return console.error(err); }
                                });
                    })
                        .catch((error) => {
                            if (error) { console.error(error); }
                                kickMember.kick()
                                    .then((member) => {
                                        message.channel.send(`${member.user.username} est kick ! Par ${message.author.username}`);
                                    })
                                        .catch((err) => {
                                            if (err) { return console.error(err); }
                                        });
                        });
        }
        if (message.content.startsWith(prefix + "mute")) {
            if (!message.guild.member(message.author).hasPermission('MUTE_MEMBERS')) { return message.channel.send('Vous n\'avez pas la permission !'); }
            if (!message.guild.member(client.user).hasPermission('MUTE_MEMBERS')) { return message.channel.send('Le bot n\'a pas la permission !'); }
            let tomute = message.guild.member(message.mentions.users.first());
            if (!tomute) { return message.channel.send('Je n\'ai pas trouvé l\'utilisateur !'); }
            let muterole = message.guild.roles.find(`name`, "Muted");
            if(!muterole){
                try{
                    muterole = await message.guild.createRole({
                        name: "Muted",
                        color: "#A9A9A9",
                        permissions: []
                    })
                    message.guild.channels.forEach(async (channel, id) => {
                        await channel.overwritePermissions(muterole, {
                            SEND_MESSAGES: false,
                            ADD_REACTIONS: false
                        });
                    });
                }catch(e){
                    console.log(e.stack);
                }
            }
            await(tomute.addRole(muterole));
            message.mentions.users.first().send(`Vous êtes mute du serveur **${message.guild.name}** par ${message.author.username}`)
            message.channel.send(`${tomute} est desormais mute par **${message.author}**`)
        }
        if (message.content.startsWith(prefix + "unmute")) {
            let tomute = message.guild.member(message.mentions.users.first());
            if (!message.guild.member(message.author).hasPermission('MUTE_MEMBERS')) { return message.channel.send('Vous n\'avez pas la permission !'); }
            if (!message.guild.member(client.user).hasPermission('MUTE_MEMBERS')) { return message.channel.send('Le bot n\'a pas la permission !'); }
            let tounmute = message.guild.member(message.mentions.users.first());
            if (!tounmute) { return message.channel.send('Je n\'ai pas trouvé l\'utilisateur !'); }
            let muterole = message.guild.roles.find(`name`, "Muted");
            await(tomute.removeRole(muterole));
            message.mentions.users.first().send(`Vous êtes unmute du serveur **${message.guild.name}** par ${message.author.username}`)
            message.channel.send(`${tomute} est desormais unmute par **${message.author}**`)
        }
        if (message.content.startsWith(prefix + "serverinfo") || message.content.startsWith(prefix + "si")) {
            try {

                function checkBots(guild) {
                let botCount = 0; 
                guild.members.forEach(member => { 
                    if(member.user.bot) botCount++;
                });
                return botCount; 
                }
            
                function checkMembers(guild) {
                let memberCount = 0;
                guild.members.forEach(member => {
                    if(!member.user.bot) memberCount++;
                });
                return memberCount;
                }
                
                if(message.author.bot) return;
                if(message.channel.type !== "text") return;
                
                let members = [];
                let indexes = [];
                
                message.guild.members.forEach(function(member){
                members.push(member.user.username);
                indexes.push(member.id);
                })
                
                let match = sm.findBestMatch(args.join(' '), members);
                let username = match.bestMatch.target;
                
                let member = message.guild.members.get(indexes[members.indexOf(username)])
                
                let definedUser = "";
                let definedUser2 = "";
                if(!args[0]) {
                    definedUser = message.author
                    definedUser2 = message.member
                } else {
                    let mention = message.mentions.users.first()
                    definedUser = mention || member.user
                    definedUser2 = message.mentions.members.first() || message.guild.members.get(args[0]) || member
                }
                const millisCreated = new Date().getTime() - message.guild.createdAt.getTime();
                const daysCreated = millisCreated / 1000 / 60 / 60 / 24;
            
                const millisJoined = new Date().getTime() - message.member.joinedAt.getTime();
            const daysJoined = millisJoined / 1000 / 60 / 60 / 24;
                let sicon = message.guild.iconURL;
                let GuildOwner = client.users.get(message.guild.ownerID)
            
                let serverembed = new Discord.RichEmbed()
                .setTitle(message.guild.name + " - Informations")
                .setColor(Math.floor(Math.random() * 16777214) + 1)
                .setThumbnail(sicon)
                .addField ("Propriétaire du serveur:", `${GuildOwner.tag}`,true)
                .addField ("ID:", `${message.guild.id}`,true)
                .addField("Membres", message.guild.memberCount, true)
                .addField('Humains:', checkMembers(message.guild), true)
                .addField('Bots:', checkBots(message.guild), true)
                .addField("Channels:", message.guild.channels.size, true)
                .addField('Niveau de vérification:', message.guild.verificationLevel, true)
                .addField("Région:", message.guild.region, true)
                .addField("Date de création:", `${moment(message.guild.createdAt).format('D/M/Y HH:mm:ss')} | ${daysCreated.toFixed(0)} jours`, true)
                .addField("Date de venue", `${moment.utc(message.member.joinedAt).format("D/M/Y, HH:mm:ss")} ${daysJoined.toFixed(0)} jours`, true )
                .addField("Rôles:", "Il y a **" + message.guild.roles.size + "** rôles.", false)
                .addField("Emojis:", "Il y a **" + message.guild.emojis.size + "** emojis.", false)
                .setFooter(client.user.username, client.user.displayAvatarURL).setTimestamp()
            
                message.channel.send(serverembed);
            
            } catch(err) {
                console.error(err);
                return message.channel.send(':x: | Une erreur c\'est produite lors du traitement de la commande.\nVeuillez envoyer un report de la commande si ce message persiste');
            };
        }
        if (message.content.startsWith(prefix + "userinfo") || message.content.startsWith(prefix + "ui")) {
            let user;

        if (!message.mentions.users.first()) user = message.author;
        else user = message.mentions.users.first();

        let date = `${user.createdAt}`.split(' ');
        let newDate = `${date[0]} ${date[2]} ${date[1]} ${date[3]} à ${date[4]}`
        let isNitro = user.premium;
        if (!isNitro) isNitro = 'Non';
        else isNitro = 'Oui';
        let status = user.presence.status;
        let color;
        let game;
        if (!user.presence.game) game = 'Aucun jeu'
        else game = user.presence.game.name;
        if (status == 'online') {
            color = 0x56f441;
            status = 'En ligne';
        } else if (status == 'dnd') {
            color = 0xff4141;
            status = 'Ne pas déranger';
        } else if (status == 'idle') {
            color = 0xff7d32;
            status = 'Inactif';
        } else {
            status = 'Absent';
        }

        message.channel.send(new Discord.RichEmbed()
            .setColor(color)
            .setThumbnail(user.avatarURL)
            .setTimestamp()
            .addField(`Compte`, `Pseudo : \`${user.username}\`\nTag : \`#${user.discriminator}\`\nDate de création : \`${newDate}\`\nNitro : \`${isNitro}\``, true)
            .addField(`Autres`, `Jeu : \`${game}\`\nID : \`${user.id}\`\nStatus : \`${status}\``, true)
        );
        }
        if (message.content.startsWith(prefix + "say")) {
            message.delete()
            message.channel.send(args.join(" "))
        }
        if (message.content.startsWith(prefix + "ping")) {
            const m = await message.channel.send("Chargement ...");
            m.edit(`**Pong !** Le temps de réponse entre toi et moi correspond à **${m.createdTimestamp - message.createdTimestamp}ms**.`);
        }
        if (message.content.startsWith(prefix + "sondage")) {
            let emd = new Discord.RichEmbed()
            .setColor("RED")
                .setTitle("Serenity - Sondage")
                .setDescription(args.join(" "))
            let msd = await message.channel.send(emd)
            msd.react("✅")
            msd.react("❌")
        }
    })
