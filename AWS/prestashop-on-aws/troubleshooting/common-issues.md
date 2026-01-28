#TROUBLESHOOTING 

Issue: Cannot connect to database 
# Check security group allows connection 
# Verify MySQL is listening on all interfaces 
sudo netstat -tlnp | grep 3306 

Issue: PrestaShop shows blank page 
# Check Apache logs 
sudo tail -f /var/log/apache2/error.log

Issue: Installation wizard not loading 
# Check file permissions 
ls -la /var/www/html 
# Should show www-data ownership
