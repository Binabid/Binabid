import discord
from discord.ext import commands
import os  # لاستدعاء متغيرات البيئة

# إعداد البوت
intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='!', intents=intents)

# عند تشغيل البوت
@bot.event
async def on_ready():
    print(f'البوت جاهز! تم تسجيل الدخول كبوت: {bot.user}')

# أمر لإنشاء تذكرة دعم فني
@bot.command()
async def تذكرة(ctx):
    guild = ctx.guild
    overwrites = {
        guild.default_role: discord.PermissionOverwrite(read_messages=False),
        ctx.author: discord.PermissionOverwrite(read_messages=True)
    }
    # إنشاء قناة جديدة باسم المستخدم لتكون تذكرة خاصة به
    channel = await guild.create_text_channel(f'تذكرة-{ctx.author.name}', overwrites=overwrites)
    await channel.send(f'مرحباً {ctx.author.mention}! سيتم الرد عليك قريباً من قبل فريق الدعم الفني.')

# أمر لإغلاق التذكرة
@bot.command()
async def اغلاق(ctx):
    if ctx.channel.name.startswith("تذكرة-"):
        await ctx.send("سيتم إغلاق التذكرة خلال 5 ثواني...")
        await ctx.channel.delete(delay=5)
    else:
        await ctx.send("هذا الأمر يمكن استخدامه فقط في قنوات التذاكر.")

# تشغيل البوت باستخدام التوكن من متغير بيئي
bot.run(os.getenv("DISCORD_TOKEN"))
