/*
discord bot:
educational bot
- can create tempory voice channels 
- move people in (assign by teacher or random)
- ask for help function (pings teacher role)
- 
*/


const Discord = require('discord.js')
require('dotenv').config()

const client = new Discord.Client();

//console.log(process.env.BOT_TOKEN)
var prefix = '!'

// variables for breakout rooms
//var numroom = 
const categoryID = '822885144107941958'
const generalID = '822645336316706838'

client.login(process.env.BOT_TOKEN)

client.on('ready', () => {
  console.log('ready')
})

client.on('message', (msg) => {
  if (msg.author.bot) return

  if (msg.content.startsWith(prefix)) {
    const [cmd_name, ...args] = msg.content
      .trim()
      .substring(prefix.length)
      .split(/\s+/)
    console.log(cmd_name)
    console.log(args) 

    switch (cmd_name) {
      case 'prefix':
        prefix = args[0]
        msg.channel.send("Changed prefix to " + prefix)
        break;

      case 'help':
        //help them out
        switch (args[0]) {
          default:
            msg.channel.send("Commands:\n!breakout create # - open rooms\n!breakout end - closes rooms\n!kick @user - kicks a user\n!ban @suer - bans a user\n!unban @user - unbans a user ") 
            break;
        }

        break;

      case 'breakout':
        if(!msg.member.hasPermission('MANAGE_CHANNELS'))
          return msg.reply('you do not have permission to do this!')

        
        else if (args[0] === "create") {
          for (let i = 0; i < args[1]; i++) { 
          let j = i + 1
            msg.guild.channels.create('Breakout Room ' +  j,
              { type: 'voice' })
              .then((channel) => {
                channel.setParent(categoryID)
              })
            }
          
          const channels = msg.guild.channels.cache.filter(c => c.parentID === categoryID &&  c.type === 'voice');
            let arr = channels.array()
            let general = msg.guild.channels.cache.filter(c => c.id === generalID);
            let connected_members = general.get(generalID).members
            connected_members.forEach((member,id) => {
              let random = Math.floor(Math.random() * args[1] + 1)
              console.log(random)
              member.voice.setChannel(arr[random])
        }) 
        }

        else if (args[0] === "end") {
          msg.channel.send("Breakout rooms closed")
          const category = msg.guild.channels.cache.filter(c =>c.id == categoryID)
          console.log(category.get(categoryID).children)
          //move people into a main room
          const channels = msg.guild.channels.cache.filter(c => c.parentID === categoryID &&  c.type === 'voice');  
          console.log(channels)
          let connected_members = category.get(categoryID).children.forEach((voiceChannel,id)=>{
            if(voiceChannel.members.size == 0){
                  voiceChannel.delete()
            }
            voiceChannel.members.forEach((member)=>{
              member.voice.setChannel(generalID)
              .then(()=>{
                  if(voiceChannel.members.size == 0){
                  voiceChannel.delete()
                }   
              }
              )
              .catch()
              console.log("moved");
            })
          })
          console.log(connected_members)
        }
      
        
        else {
        msg.channel.send("Command usage is\n!breakout [random/optional/end] [# of rooms]") 
        }  
      break;

    
      case 'ban':
        if (!msg.member.hasPermission('BAN_MEMBERS'))
          return msg.reply('You do not have permission to ban users.')
        if (args.length === 0) return msg.reply('Provide a User ID.')
        msg.mentions.members.forEach(member => member.ban()
          .then((id) =>
            msg.channel.send("User has been banned!"))
          .catch((err) => msg.channel.send ('I do not have permission to ban this user.')))
        break;

      case 'unban': 
        let userID = args[0]
        userID = userID.replace(/[\\<>@#&!]/g, "")
        if (userID == '') return msg.reply('provide a user ID')
        if(userID){
        msg.guild.fetchBans().then(member => member.unban()) 
        
            msg.guild.members.unban(userID)
            msg.channel.send('User has been unbanned.')
          .catch(()=> console.error)
        }
        
      else return msg.reply("provide a valid user ID.")
      break;
      case 'kick':
        if (!msg.member.hasPermission('KICK_MEMBERS'))
          return msg.reply('You do not have permission to kick users.')
        if (args.length === 0) return msg.reply('provide a user ID.')
        msg.mentions.members.forEach(member => member.kick()
          .then((id) =>
            msg.channel.send("User has been kicked!"))
          .catch((err) => msg.channel.send ('I do not have permission to kick this user.')))
        break;
    }
  }
})
