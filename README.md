# SchedulePowerOnOff
关键代码说明：
    
    
    
    
    
    
    /**
     * Used in pre-L devices, where "next alarm" is stored in system settings.
     * 修改系统文件中下一个闹钟时间
     */
    @SuppressWarnings("deprecation")
    @TargetApi(Build.VERSION_CODES.KITKAT)
    public static void updateNextAlarmInSystemSettings(Context context, Calendar nextAlarm) {
        String time = "";
        if (nextAlarm != null) {
            time =  getFormattedTime(context, nextAlarm);
        }
        try {
            Settings.System.putString(context.getContentResolver(), NEXT_ALARM_FORMATTED, time);

        } catch (SecurityException se) {
        }
    }

    public static String getFormattedTime(Context context, Calendar time) {
        final String skeleton = DateFormat.is24HourFormat(context) ? "EHm" : "Ehma";
        final String pattern = DateFormat.getBestDateTimePattern(Locale.getDefault(), skeleton);
        return (String) DateFormat.format(pattern, time);
    }
    /**
     * 定时开机
     * @param context
     * @param nextStartTime
     */
    public static void powerOn(Context context, long nextStartTime){
        Intent intent = new Intent("org.codeaurora.poweroffalarm.action.SET_ALARM");
        intent.addFlags(Intent.FLAG_RECEIVER_FOREGROUND);
        intent.setPackage("com.qualcomm.qti.poweroffalarm");
        intent.putExtra("time", nextStartTime);
        context.sendBroadcast(intent);
        Calendar calendar = Calendar.getInstance(TimeZone.getDefault());
        calendar.setTimeInMillis(nextStartTime);
        updateNextAlarmInSystemSettings(context,calendar);
        
    }
